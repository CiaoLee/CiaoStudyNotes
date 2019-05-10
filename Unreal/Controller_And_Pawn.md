# **Controller And Pawn**

## **How Controller And Character Spawn In Level**

### **Call Sequence**

- UGameEngine::Tick( float Deltaseconds, bool bIdleMode )
- UEngine::TickWorldTravel( FWorldContext& WorldContext, float DeltaSeconds )
- UEngine::Browse( FWorldContext& WorldContext, FURL URL, FString& Error )
- UEngine::LoadMap(  FWorldContext& WorldContext, FURL URL, UPendingNetGame* Pending ,FString& Error )
- ULocalPlayer::SpawnPlayActor(const FString& URL, FString& OutError, UWorld* InWorld);

```c++
bool ULocalPlayer::SpawnPlayActor(const FString& URL, FString& OutError, UWorld* InWorld)
{
    check(InWorld);
    if(InWorld->IsServer())
    {
        FURL PlayerURL(NULL,*URL,TRAVEL_Absolute);

        //Get player nickname.
        FString PlayerName =  GetNickName();

        if(PlayerName.Len()>0)
        {
            PlayerURL.AddOption(*FString::Printf(TEXT("NAME=%s"),*PlayerName));
        }

        //Send any game-specific url options for this player;
        FString GameUrlOptions =  GetGameLoginOptions();
        if(GameUrlOptions.Len() > 0)
        {
            PlayerURL.AddOption(*FString::Printf(TEXT("%s"),*GameUrlOptions));
        }

        //Get player unique id
        FUniqueNetIdRepl UniqueId(GetPreferredUniqueNetId());

        PlayerController = InWorld->SpawnPlayActor(this,ROLE_SimulatedProxy,PlayerURL, UniqueId, OutError, GEngine->GetGamePlayers(InWorld).Find(this));
    }else
    {

        //Statically bind to the specified player controller
        UClass* PCClass = PendingLevelPlayerControllerClass;

        //The playerController gets replicated from the client though
        //the engine assumes that every Player always has a valid PlayerController so we spawn
        // a dummy one that is going to be replaced later.

        //Look at APlayerController::OnActorChannelOpen + UNetConnection::HandleClientPlayer for the code that
        //replaces this fake player controller with the real replicated one from the server

        FActorSpawnParameters SpawnInfo;

        // We never want to save player controllers into a map.
        SpawnInfo.ObjectFlags |= RF_Transient;
        PlayerController = InWorld->SpawnActor<APlayerController>(PCClass, SpawnInfo);
        const int32 PlayerIndex = GEngine->GetGamePlayers
    }
}
```

- UWorld::SpawnPlayActor(UPlayer* NewPlayer, ENetRole RemoteRole, const FURL& InURL, const FUniqueNetIdRepl& UniqueId, FString& Error,uint8 InNetPlayerIndex);

```C++
APlayerController* UWorld::SpawnPlayActor(UPlayer* NewPlayer, ENetRole RemoteRole, const FURL& InURL, const FUniqueNetId& UniqueId, FString& Error,uint8 InNetPlayerIndex)
{
    //Make the option string

    if(AGameModeBase* const GameMode = GetAuthGameMode())
    {
        //Give the GameMode a chance to accept the login
        APlayerController* const NewPlayerController = GameMode->Login(NewPlayer, RemoteRole, *InURL, Options, UniqueId, Error);

        if(NewPlayerController == NULL)
        {
            
        }

    }
}
```


  