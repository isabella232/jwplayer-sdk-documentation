# Enable Google IMA

<img src="https://img.shields.io/badge/SDK-iOS%20v3-0AAC29.svg?logo=android">

<sup>Last Updated: August 6, 2019</sup>

The JW Player SDK for iOS integrates Google's IMA iOS SDK. With this SDK integration, you can use the Google IMA ad client to request ads.

<br/>

## Add the Google IMA dependency

To begin using the Google IMA ad client, you must first add a dependency to your app.

### CocoaPods

#### Edit Podfile

1. In a text editor, open the **Podfile** for your app.
2. Add `GoogleAds-IMA-iOS-SDK` as a dependency. Be sure to use IMA SDK version located in the <a href="https://developer.jwplayer.com/sdk/ios/reference/Classes/JWPlayerController.html#//api/name/googleIMAVersion" target="_blank">googleIMAVersion</a> property. You can also review this <a href="https://developer.jwplayer.com/sdk/ios/docs/developer-guide/#plugin-support" target="_blank">plugin support table</a>.
3. Save **Podfile** and close the text editor.

```groovy
# Pods for MyAwesomeProject
pod 'JWPlayer-SDK', '~> 3.6'
pod 'GoogleAds-IMA-iOS-SDK', '~> 3.8.1'
```

#### Install SDK

1. At the terminal prompt of your project directory, enter `$ pod install` to install the JW Player SDK for iOS.
2. Open the **.xcworkspace** file for your project to launch Xcode.

### Local

Now that you have the required items listed in the previous subsection, you can add JW Player SDK for iOS to your project.

1. Download and unzip the Google IMA iOS SDK.
2. From within Xcode, expand the project in the navigator.
3. Drag the **GoogleInteractiveMediaAds.framework** file from your desktop into the **Frameworks** folder in Xcode. In the pop-up screen that appears, be sure to select your target.
4. Click **Finish**.
5. Select the target in the project editor.
6. Click General.
7. Verify that the **GoogleInteractiveMediaAds.framework** appears in both the **Embedded Binaries** and **Linked Frameworks and Libraries** sections. <br/><br/>If the framework does not appear in both sections, delete the instance of the framework. Then, drag the **GoogleInteractiveMediaAds.framework** folder from the Xcode navigation to the **Embedded Binaries** section. The framework should appear in both sections.

## Add a pre-roll ad to a player

Use the following steps to add a pre-roll ad to the player you added to your view:

1. Instantiate a <a href="https://developer.jwplayer.com/sdk/ios/reference/Classes/JWAdBreak.html" target="_blank">JWAdBreak</a> object called `adBreak`. At a minimum, you must assign an ad tag URL to the `tag` property.
2. Instantiate a <a href="https://developer.jwplayer.com/sdk/ios/reference/Classes/JWAdConfig.html" target="_blank">JWAdConfig</a> object and assign it to `config.advertising`.
3. Define `config.advertising.client` as `JWAdClientGoogima` (Obj-C) or `.googima` (Swift). This defines the ad client.
4. Add `adBreak` to the <a href="https://developer.jwplayer.com/sdk/ios/reference/Classes/JWAdConfig.html#//api/name/schedule" target="_blank">schedule</a> array property of the `JWAdConfig`. This adds the ad schedule to the player's `config` property.

```Obj-C
...

@property (nonatomic) JWPlayerController *player;
@property (nonatomic, weak) IBOutlet UIView *playerContainerView;
@end

@implementation ObjCViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    JWAdBreak *adBreak = [JWAdBreak initWithTags:@"https://www.domain.com/adtag.xml" offset:@"pre"];

    JWConfig *config = [JWConfig configWithContentURL:@"http://example.com/hls.m3u8"];
    config.advertising = [JWAdConfig new];
    config.advertisting.client = JWAdClientGoogima;
    config.advertising.schedule = @[adBreak];
    self.player = [[JWPlayerController alloc] initWithConfig:config];
}

- (void)viewDidAppear {
    [super viewDidAppear];
    [self.view addSubview:self.player.view];
}

...
```
```Swift
class ViewController: UIViewController {
    @IBOutlet weak var playerContainerView: UIView!
    var player: JWPlayerController?

    override func viewDidLoad() {
        super.viewDidLoad()

        var adBreak = JWAdBreak(tag: "https://www.domain.com/adtag.xml", offset: "pre")

        var config = JWConfig(contentURL: "http://example.com/hls.m3u8")
        config.advertising = JWAdConfig()
        config.advertising.client = .googima
        config.advertising.schedule = [adBreak]
        player = JWPlayerController(config: config)
    }

    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        playerContainerView.addSubview(player!.view)
    }
}
```
You can build upon this basic implementation by [adding more ad breaks](#add-multiple-ad-breaks-to-a-player).

<br/>

<a name="add-multiple-ad-breaks-to-a-player"></a> 

## Add multiple ad breaks to a player

Use the following steps to add multiple ad breaks to the previous pre-roll example:

1. Instantiate an additional `JWAdBreak` object.
2. Assign an ad tag to the `tag` property.
3. When defining the <a href="https://developer.jwplayer.com/sdk/ios/reference/Classes/JWAdBreak.html#//api/name/offset" target="_blank">offset</a> property, choose one of the following values to schedule a mid-roll or post-roll ad:<br/><br/>**Mid-roll**<br/>&nbsp;&nbsp;- **{number}**: (String) Ad plays after the specified number of seconds.<br/>&nbsp;&nbsp;- **{timecode}**: (String) Ad plays at a specific time, in `hh:mm:ss:mmm` format.<br/>&nbsp;&nbsp;- **{xx%}**: (String) Ad plays after xx% of the content has played.<br/><br/>**Post-roll**<br/>&nbsp;&nbsp;- `post`: (String) Ad plays after the content.<br/><br/>
4. Add the additional `AdBreak` object to the `schedule` array.

```Obj-C
@property (nonatomic) JWPlayerController *player;
@property (nonatomic, weak) IBOutlet UIView *playerContainerView;
@end

@implementation ObjCViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    JWAdBreak *adBreak = [JWAdBreak initWithTags:@"https://www.domain.com/adtag.xml" offset:@"pre"];

    JWAdBreak *adBreak2 = [JWAdBreak initWithTags:@"https://www.domain.com/adtag-mid-roll1.xml" offset:@"10"];

    JWAdBreak *adBreak3 = [JWAdBreak initWithTags:@"https://www.domain.com/adtag-mid-roll2.xml" offset:@"00:00:15:000"];

    JWAdBreak *adBreak4 = [JWAdBreak initWithTags:@"https://www.domain.com/adtag-mid-roll3.xml" offset:@"25%"];

    JWAdBreak *adBreak5 = [JWAdBreak initWithTags:@"https://www.domain.com/adtag-post-roll.xml" offset:@"post"];

    JWConfig *config = [JWConfig configWithContentURL:@"https://example.com/hls.m3u8"];
    config.advertising = [JWAdConfig new];
    config.advertising.client = JWAdClientGoogima;
    config.advertising.schedule = @[adBreak,
        adBreak2,
        adBreak3,
        adBreak4,
        adBreak5];
    self.player = [[JWPlayerController alloc] initWithConfig:config];
}

- (void)viewDidAppear {
    [super viewDidAppear];
    [self.view addSubview:self.player.view];
}
```
```Swift
class ViewController: UIViewController {
    @IBOutlet weak var playerContainerView: UIView!
    var player: JWPlayerController?

    override func viewDidLoad() {
        super.viewDidLoad()

        var adBreak = JWAdBreak(tag: "https://www.domain.com/adtag.xml", offset: "pre")

        var adBreak2 = JWAdBreak(tag: "https://www.domain.com/adtag-mid-roll1.xml", offset: "10")

        var adBreak3 = JWAdBreak(tag: "https://www.domain.com/adtag-mid-roll2.xml", offset: "00:00:15:000")

        var adBreak4 = JWAdBreak(tag: "https://www.domain.com/adtag-mid-roll3.xml", offset: "25%")

        var adBreak5 = JWAdBreak(tag: "https://www.domain.com/adtag-post-roll.xml", offset: "post")

        var config = JWConfig(contentURL: "https://example.com/hls.m3u8")
        config.advertising = JWAdConfig()
        config.advertising.client = .googima
        config.advertising.schedule = [adBreak, adBreak2, adBreak3, adBreak4, adBreak5]
        player = JWPlayerController(config: config)
    }

    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        playerContainerView.addSubview(player!.view)
    }
}
```



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
