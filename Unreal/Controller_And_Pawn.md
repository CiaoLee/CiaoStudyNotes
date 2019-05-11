# **Controller And Pawn**

## **How Controller And Character Spawn In Level**

### **Call Sequence**

- UGameEngine::Tick( float Deltaseconds, bool bIdleMode )
- UEngine::TickWorldTravel( FWorldContext& WorldContext, float DeltaSeconds )
- UEngine::Browse( FWorldContext& WorldContext, FURL URL, FString& Error )

```C++

```

- UEngine::LoadMap(  FWorldContext& WorldContext, FURL URL, UPendingNetGame* Pending ,FString& Error )

```C++

bool UEngine::LoadMap(FWorldContext& WorldContext,FURL URL, class UPendingNetGame* Pending, FString& Error)
{

    //....
    //....
    //Spawn play actors for all active for all local players.
    if(WorldContext.OwningGameInstance != NULL)
    {
        for(auto It =WorldContext.OwningGameInstance->GetLocalPlayerIterator();It;++It)
        {
            FString Error2;
            if(!(*It)->SpawnPlayActor(URL.ToString(1),Error2,WorldContext.World()))
            {
                UE_LOG(LogEngine,Fatal,TEXT("Couldn't spawn player:%s"),*Error2);
            }
        }

    }

    //Prime texture streaming.
    IStreamingManager::Get().NotifyLevelChange();

    //Set VRSystem OnBeginPlay
    if(GEngine && GEngine->XRSystem.IsValid())
    {
        GEngine->XRSystem->OnBeginPlay(WorldContext);
    }

    //!!!!The Point call BeginPlay();
    WorldContext.World()->BeginPlay();

    //World PostLoad CallBack.
    FCoreUObjectDelegates::PostLoadMapWithWorld.Broadcast(WorldContext.World();
    WorldContext.World()->bWorldWasLoadedThisTick = true;

    //We want to update streaming immediately so that there's no tick prior to
    //RedrawViewports

    if(WorldContext.World()->GetAuthGameMode() == NULL || WorldContext.World()->GetAuthGameMode()->IsHandlingReplays())
    {
        if(URL.HasOption(TEXT("DemoRec")) && WorldContext.OwningGameInstance != nullptr)
        {
            const TCHAR* DemoRecName =  URL.GetOption(TEXT("DemoRec="),NULL);

            //Record the demo, optionally with the specified custom name.
            WorldContext.OwningGameInstance->StartRecordingReplay(FString(DemoRecName),WorldContext.World()->GetMapName(),URL.Op);
        }
    }
}

```

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
        const int32 PlayerIndex = GEngine->GetGamePlayers(InWorld).Find(this);
        PlayerController->NetPlayerIndex = PlayerIndex;
        PlayerController->Player = this;
    }
    return PlayerController != NULL;
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
            UE_LOG(LogSpawn,Warning,TEXT("Login failed:%s"),*Error);
            return NULL;
        }

        UE_LOG(LogSpawn,Log,TEXT("%s got player %s[%s]"),*NewPlayerController->GetName(),*NewPlayer->GetName(),UniqueId.IsValid()?*UniqueId->ToString():TEXT("InValid"));

        //Possess the newly-spawned player;
        NewPlayerController->NetPlayerIndex = InNetPlayerIndex;
        NewPlayerController->Role = ROLE_Authority;
        NewPlayerController->SetReplicates(RemoteRole!= ROLE_None);
        if(RemoveRole == ROLE_AutonomousProxy)
        {
            NewPlayerController->SetAutonomousProxy(true);
        }
        NewPlayerController->SetPlayer(NewPlayer);
        GameMode->PostLogin(NewPlayerController);
        return NewPlayerController;
    }
}
```
- APlayerController* AGameModeBase::


  