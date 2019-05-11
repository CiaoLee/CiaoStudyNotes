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

- void UEngine::TickWorldTravel(FWorldContext& Context, float DeltaSeconds)
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

    //Handle clinet traveling.
}