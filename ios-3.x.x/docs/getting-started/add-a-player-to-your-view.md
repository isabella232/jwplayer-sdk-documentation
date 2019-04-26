# Add a player to your view

<img src="https://img.shields.io/badge/SDK-iOS%20v3-0AAC29.svg?logo=apple">

<sup>Last Updated: April 26, 2019</sup>

Use the following steps and code examples to add a player to the **ViewController.h** or **ViewController.swift** file of your app:

<br/>

1. In **ViewController.h** or **ViewController.swift**, create a `JWPlayerController` object named `player`.
2. Create a `JWConfig` object named `config`. At the minimum, *you must define* the `file` property.
4. Initialize the `player` with the `config`.

```Objective-C
@property (nonatomic) JWPlayerController *player;

@end

@implementation ObjCViewController


- (void)viewDidLoad {
    [super viewDidLoad];

    JWConfig *config = [[JWConfig alloc] init];
    config.file = @"http://example.com/hls.m3u8";
    self.player = [[JWPlayerController alloc] initWithConfig:config];
}

- (void)viewDidAppear {
    [super viewDidAppear];
    [self.view addSubview:self.player.view];
}
```

```Swift
class ViewController: UIViewController {
    var player: JWPlayerController?

    override func viewDidLoad() {
        super.viewDidLoad()
        let config: JWConfig  = JWConfig()
        config.file = "http://example.com/hls.m3u8"
        player = JWPlayerController(config: config)
    }

    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        view.addSubview(player!.view)
    }
}
```

