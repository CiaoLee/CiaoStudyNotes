# **GEngine Tick**

## **How GEngine Tick Sequence**

### **Call Sequence**

- void UGameEngine::Tick(float DeltaSeconds, bool bIdelMode);
```C++

void UGameEngine::Tick(float DeltaSeconds,bool bIdleMode)
{
    //Iterate all context.
    {
        TickWorldTravel(Context,DeletaSeconds);

        if(!bIdleMode)
        {
            Context.World()->Tick(LEVELTICK_ALL,DeltaSeconds);
        }
    }

    //------------------------------------------------------
    // End per-world ticking
    //------------------------------------------------------
    {
        FTickableGameObject::TickObjects(nullptr,LEVELTICK_All,false,DeltaSeconds);
    }
    //Call the render.
    {
        //Render everything.
        RedrawViewports();

        // Some tasks can only be done we finish all scene/viewports.
        GetRendereModule().PostRenderAllViewports();
    }
    if(GIsClient)
    {
        //Update resource streaming after viewports have had a chance to update view imformation.Normal update.
        IStreamingManager::Get().Tick(DeltaSeconds);
    }

    //Update Audio.
    FAudioDeviceManager* GameAudioDeviceManager =  GEngine->GetAudioDeviceManager();
    
    if(GameAudioDeviceManager)
    {
        GameAudioDeviceManager->UpdateActiveAudioDevices(bIsAnyNonPreviewWorldUnPaused);
    }

    //rendering thread commands.
    {
        GetRenderingModule().TickRenderTargetPool();
    }
}
```

- bool UEngine::LoadMap(FWorldContext& WorldContext, FURL URL, class UPendingNetGame* Pending, FString& Error);

```C++
bool UEngine::LoadMap(FWorldContext& WorldContext,FURL URL, class UPendingNetGame* Pending, FString& Error)
{
    //Upload the current world
    if(WorldContext.World())
    {

        //trim memory to clear up allocations from the previous level(also flushes rendering)

        //Cancels the Forced StreamType for textures using a timer.
        if(!IStreamingManager::HasShutdown())
        {
            IStreamingManager::Get().CancelForcedResources();
        }



        
    }
    
    //......................

    //Preload Content for URL.
    WorldContext.OwningGameInstance->PreloadContentForURL(URL);
    UPackage* WorldPackage = NULL;
    UWorld* NewWorld = NULL;

    //NormalMapLoading....
    
    NewWorld->SetGameInstance(WorldContext.OwningGameInstance);

    GWorld = NewWorld;

    WorldContext.SetCurrentWorld(NewWorld);
    WorldContext.World()->WorldType = WorldContext.WorldType.

    //InitWorld Call.
    
}

```

- void UEngine::TickWorldTravel(FWorldContext& Context, float DeltaSeconds)

```C++
{

    //Handle Seamless Traveling
    if(Context.SeamlessTravelHandler.IsInTransition())
    {
        //Note: SeamlessTravelHandler.Tick may automatically update Context.World and GWorld internally.
        Context.SeamlessTravelHandler.Tick();
    }

    //Handle server traveling.
    if(Context.World()==nullptr)
    {
        UE_LOG(LogLoad,TEXT("UEngine::TickWorldTravel has no world."))
   		BrowseToDefaultMap(Context);
		BroadcastTravelFailure(Context.World(), ETravelFailure::ServerTravelFailure, TEXT("UEngine::TickWorldTravel has no world after ticking seamless travel handler."));
		return;
    }

    //If has next world switch to next.
    {
        //.....
        EBrowseReturnVal::Type Ret = Browse(Context,FURL(&Context.LastURL,*NextURL,(ETravelType)Context.World()->NextTravelType),Error);
        //Handle unsuccess.
    }

    //Handle client traveling.
    if(!Context.TravelURL.IsEmpty())
    {
        AGamemode* const GameMode = Context.World()->GetAuthGameMode();
        if(GameMode)
        {
            GameMode->StartToLeaveMap();
        }

        FString Error,TravelURLCopy = Context.TravelURL;
        if(Browse(Context,FURL(&Context.LastURL,*TravelURLCopy,(ETravelType)Context.TravelType),Error)==EBrowseReturnVal::Failure)
        {
            if(Context.World()==NULL)
            {
                BrowseToDefaultMap(Context);
            }

            //Let people know that we failed to client travel
            BroadcastTravelFailure(Context.World(),ETravelFailure::ClientTravelFailure,Error);
        }

        check(Context.World() != NULL);
        return;
    }
    return;
}
```
