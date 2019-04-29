# **Controller And Pawn**

## **How Controller And Character Spawn In Level**

### **Call Sequence**

- UGameEngine::Tick( float Deltaseconds, bool bIdleMode )
- UEngine::TickWorldTravel( FWorldContext& WorldContext, float DeltaSeconds )
- UEngine::Browse( FWorldContext& WorldContext, FURL URL, FString& Error )
- UEngine::LoadMap(  FWorldContext& WorldContext, FURL URL, UPendingNetGame* Pending ,FString& Error )
- ULocalPlayer::SpawnPlayActor(const FString& URL, FString& OutError, UWorld* InWorld)
- 