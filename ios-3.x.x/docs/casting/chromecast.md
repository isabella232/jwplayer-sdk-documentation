# Enable casting to Chromecast devices

<sup>Last Updated: March 14, 2019

The Google Cast framework enables a viewer to stream video and audio content to a compatible TV or sound system. By enabling the Google Cast framework in your app, a viewer can use a cast button to stream your content to a Chromecast-enabled device on a shared network connection.

!!!important
&bull; The JW Player iOS SDK supports casting to the Default Media Receiver and to Styled Media Receivers.<br/><br/>&bull; Custom receivers are not officially supported. However, if the video playback implements the same interface used in the Default Media Receiver, you may be able to initiate a casting session with a custom receiver.<br/><br/>&bull; To specify a receiver, set a media receiver app ID to the `chromeCastReceiverAppID` property of the [JWCastController](https://staging-developer.jwplayer.com/sdk/ios/reference/Classes/JWCastController.html).
!!!

**1.** In a text editor, open **Podfile**.

<br/>

**2.** If you are using CocoaPods manage dependencies, add `google-cast-sdk` to your **Podfile**, as shown in the following code example. You can specify any Google Cast version that is greater than or equal to 4.3, but less than 5.0.  

If you are manually managing dependencies for your project, follow these <a href="https://developers.google.com/cast/docs/ios_sender/#manual_setup" target="_blank">manual setup instructions</a>.

```groovy
# Uncomment the next line to define a global platform for your project
platform :ios, '11.0'

target 'MyAwesomeProject' do
    # Comment the next line if you're not using Swift and don't want to use dynamic frameworks
    use_frameworks!

    # Pods for MyAwesomeProject
    pod 'JWPlayer-SDK', '~> 3.0'
    pod 'google-cast-sdk', '~> 4.3' 
end
```

<br/>

**3.** If you are using Xcode 10 and targeting iOS 12+, click the app target **> Capabilities > Access WiFi Information**. Click the toggle to **ON** to enable **Access Wifi Information**.

<br/>

**4.** In your app, create a `JWCastController` object and set its delegate. The delegate must adhere to the [JWCastingDelegate](https://developer.jwplayer.com/sdk/ios/reference/Protocols/JWCastingDelegate.html) protocol and implement its delegate methods.

```Objective-C
- (void)setUpCastController
{
    self.castController = [[JWCastController alloc] initWithPlayer:self.player];
    self.castController.chromeCastReceiverAppID = kGCKMediaDefaultReceiverApplicationID;
    self.castController.delegate = self;
}
```
```Swift
func setUpCastController() {
    castController = JWCastController(player: player)
    castController?.chromeCastReceiverAppID = kGCKMediaDefaultReceiverApplicationID
    castController?.delegate = self
}
```


<br/>

**5.** Call the `scanForDevices` method to scan for devices: `[self.castController scanForDevices];`. When devices become available, the `JWCastingDelegate` method, `onCastingDevicesAvailable:`, is called and returns an array of `JWCastingDevices`.

<br/>

**6.** Call the `connectToDevice:` method to connect to a device.  When a connection is established, the `JWCastingDelegate` method, `onConnectedToCastingDevice:`, is called. This signals the ability to cast the video that is reproduced by the `JWPlayerController`.

```Objective-C
-(void)onCastingDevicesAvailable:(NSArray *)devices
{
    JWCastingDevice *chosenDevice = devices[0];
    [self.castController connectToDevice:chosenDevice];
}
```
```Swift
func onCastingDevicesAvailable(_ devices: [JWCastingDevice]!) 
{
    let chosenDevice = devices.first
    castController?.connect(to: chosenDevice)
}
```

<br/>

**7.** Call the `cast` method on your `JWCastController`.

```Objective-C
-(void)onConnectedToCastingDevice:(JWCastingDevice *)device
{
    [self.castController cast];
}
```
```Swift
func onConnected(to device: JWCastingDevice?) {
    castController?.cast()
}
```

<br/>

The `JWPlayerController` API controls the playback of the video being casted, and the JWPlayerDelegate will provide you with the playback callbacks while casting.

!!!tip
The JW Player iOS Best Practice Apps repository an <a href="https://github.com/jwplayer/jwplayer-ios-bestPracticeApps/tree/master/JWBestPracticeApps/JWCasting" target="_blank">example casting implementation</a>.contains several best practice apps, including an example of the code necessary for a casting experience.<br/><br/>Additionally, you can learn more about the Google Cast <a href="https://developers.google.com/cast/docs/ux_guidelines" target="_blank">User Experience</a> guidelines.
!!!

<br/>

## FAQ

**Which features not supported when casting with an iOS SDK player?**
<br/><br/>
The following features are not supported during a casting session with an iOS SDK player:

* Google IMA ads
* Multiple-audio tracks or AudioTrack switching<sup>1</sup>
* In-manifest WebVTT captions<sup>1</sup>
* 608 captions
* DVR and live streaming capabilities

<sup>1</sup>Chromecast does not support these features natively.

<br/><br/>
<div id="wufoo-mff60sc1xnn4cu">
Use this <a href="https://jwplayerdocs.wufoo.com/forms/mff60sc1xnn4cu">form</a> to provide your feedback.
</div>
<script type="text/javascript">var mff60sc1xnn4cu;(function(d, t) {
var s = d.createElement(t), options = {
'userName':'jwplayerdocs',
'formHash':'mff60sc1xnn4cu',
'autoResize':true,
'height':'288',
'async':true,
'host':'wufoo.com',
'header':'show',
'ssl':true,
'defaultValues': 'field118=' + location.pathname};
s.src = ('https:' == d.location.protocol ? 'https://' : 'http://') + 'www.wufoo.com/scripts/embed/form.js';
s.onload = s.onreadystatechange = function() {
var rs = this.readyState; if (rs) if (rs != 'complete') if (rs != 'loaded') return;
try { mff60sc1xnn4cu = new WufooForm();mff60sc1xnn4cu.initialize(options);mff60sc1xnn4cu.display(); } catch (e) {}};
var scr = d.getElementsByTagName(t)[0], par = scr.parentNode; par.insertBefore(s, scr);
})(document, 'script');</script>
