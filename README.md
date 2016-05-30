# AllJoynClientLib
An Universal Windows apps (UWP) Device Client Library for various common AllJoyn devices

Supported AllJoyn devices:

- Lighting Service Framework (smart lights, like LIFX or Philips Hue using [My Hue DSB Bridge](https://github.com/dotMorten/AllJoynPhilipsHueDSB))
- [AllPlay](https://www.qualcomm.com/products/allplay/platform) media players like [Gramofon](https://gramofon.com/) and [Panasonic All](http://www.panasonic.com/uk/consumer/home-entertainment/wireless-speaker-systems.html)
- Z-Wave switches using [Microsoft's Z-Wave Device Service Bridge](https://developer.microsoft.com/en-us/windows/iot/win10/samples/zwavetutorial)



### Usage:
```csharp
  
    var manager = new AllJoynClientLib.DeviceManager();
    manager.DeviceJoined += Manager_DeviceJoined;
    manager.Start();
    
    // ...

    private async void Manager_DeviceJoined(object sender, DeviceClient device)
    {
      if(device is AllJoynClientLib.Devices.LSF.LightClient)
      { //We found a light
        var client = (AllJoynClientLib.Devices.LSF.LightClient)device;
        await client.SetOnOffAsync(true); //turn on light
        if(await GetIsColorSupportedAsync()) //check the capability of light
          await client.SetColorAsync(Colors.Red); //Set the color of the light
      }
      else if (device is AllJoynClientLib.Devices.AllPlay.PlayerClient)
      { //We found a media player
        var client = (AllJoynClientLib.Devices.AllPlay.PlayerClient)device;
        await client.MediaPlayer.NextAsync(); //Play next track
        var list = await client.MediaPlayer.GetPlaylistAsync(); //Get playlist
        await client.Volume.SetVolumeAsync(50); //Set volume
        client.MediaPlayer.PlayStateChanged += OnPlayStateChanged;
      }
    }

    private void OnPlayStateChanged(object sender, AllJoynClientLib.Devices.AllPlay.PlayState e)
    {
        //Media play state changed (start/stop/buffering/transitioning etc)
        string currentSong = e.CurrentMedia.Title;
    }
```

See the test app for more examples


### Sample App Screenshots
![image](https://cloud.githubusercontent.com/assets/1378165/15642669/aa6ccdc4-25fd-11e6-9d1e-79168715a183.png)
![image](https://cloud.githubusercontent.com/assets/1378165/15642681/d0b0fc4e-25fd-11e6-94bf-da701a03f32d.png)
![image](https://cloud.githubusercontent.com/assets/1378165/15642715/fbc284c0-25fd-11e6-9bb4-b277a406e067.png)

