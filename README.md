<h1 align="center">iLuck</h1>
<p align="center">
    <a href="https://github.com/rios0rios0/iluck/releases/latest">
        <img src="https://img.shields.io/github/release/rios0rios0/iluck.svg?style=for-the-badge&logo=github" alt="Latest Release"/></a>
    <a href="https://github.com/rios0rios0/iluck/blob/main/LICENSE">
        <img src="https://img.shields.io/github/license/rios0rios0/iluck.svg?style=for-the-badge&logo=github" alt="License"/></a>
</p>

An iOS slot machine game built with Objective-C where players spin a three-column picker to match tech company logos (Apple, Mac, Microsoft, Windows, Google, Android). The game tracks attempts and plays sound effects for spins and wins, providing a simple but complete casual gaming experience with custom UI assets and audio feedback. Development discontinued on 2012-04-12.

## Features

- **Three-column `UIPickerView` slot machine** -- each column displays tech company logos as image views, spinning independently on each play
- **Win detection** -- matches when all three columns land on the same logo; displays a congratulatory alert with the number of attempts it took to win
- **Sound effects** -- rotation sound on each spin and a winner sound on match, implemented with `AudioToolbox` (`AudioServicesPlaySystemSound`)
- **Random number generation** -- seeded with `srandom(time(NULL))` for unpredictable spins
- **Custom image-based picker** -- renders `UIImageView` objects in the picker via `pickerView:viewForRow:forComponent:reusingView:` delegate method
- **Action sheet menu** -- "More Apps" and "About This App" options
- **Button state management** -- the Play button is disabled during animation and re-enabled after a delay to prevent rapid re-spins
- **Pre-built IPA** -- includes a compiled `.ipa` file and `.app` bundle in the `Build/` directory

## Game Mechanics

1. Press the **Play** button to spin all three columns
2. Each column randomly selects one of 6 tech logos: Apple, Mac, Microsoft, Windows, Google, Android
3. If all three columns show the same logo, you win
4. A winner sound plays and an alert shows "You Win After X Attempts"
5. The attempt counter persists until the app is closed

## Technologies

- **Objective-C** (manual reference counting)
- **Xcode** with Interface Builder (XIB)
- **UIKit** -- `UIPickerView`, `UIButton`, `UIActionSheet`, `UIAlertView`
- **AudioToolbox** -- `AudioServicesCreateSystemSoundID`, `AudioServicesPlaySystemSound`
- **iOS SDK 5.x** (deployment target era)

## Project Structure

```
iluck/
├── Build/
│   ├── iLuck.app/                # Pre-built application bundle
│   │   ├── iLuck                 # Compiled binary
│   │   ├── Apple.png             # Picker images
│   │   ├── Android.png
│   │   ├── Google.png
│   │   ├── Mac.png
│   │   ├── Microsoft.png
│   │   ├── Windows.png
│   │   ├── Background.png
│   │   ├── Button Normal.png
│   │   ├── Button Selected.png
│   │   ├── Play.wav              # Spin sound effect
│   │   ├── Winner.wav            # Win sound effect
│   │   └── Show Info.wav
│   └── iLuck.ipa                 # Packaged iOS application
├── Imgs/
│   ├── Backgrounds/              # Background and splash screen PSD/PNG sources
│   ├── Buttons/                  # Button state images
│   ├── Icons/                    # App icons (multiple sizes + PSD sources)
│   └── PickerView/               # Tech company logo PNGs for the slot machine columns
├── Project/
│   ├── iLuck.xcodeproj/
│   └── iLuck/
│       ├── AppDelegate.{h,m}
│       ├── MasterViewController.{h,m}   # Main game controller with picker logic
│       ├── MainWindow.xib                # Main window with picker and play button
│       ├── main.m
│       ├── Play.wav
│       ├── Winner.wav
│       └── *.png                         # Runtime image assets
└── LICENSE
```

## Installation

1. Install **Xcode** (originally developed with Xcode 4.x)
2. Open `Project/iLuck.xcodeproj`
3. Select an iOS Simulator target and run (Cmd+R)

> **Note:** This project uses manual reference counting (MRC). Modern Xcode may require `-fno-objc-arc` compiler flags.

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

This project is licensed under the terms specified in the [LICENSE](LICENSE) file.
