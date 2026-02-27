# Contributing

> **This project was discontinued in April 2012 and is no longer actively maintained.**
> The repository is preserved as a historical reference. No new features or bug fixes are planned.

## Historical Build Information

This project was built using the following tools and technologies:

- **Language:** Objective-C (manual reference counting / MRC)
- **Platform:** iOS (iPhone), iOS SDK 5.x
- **Frameworks:** UIKit (`UIPickerView`, `UIButton`, `UIActionSheet`, `UIAlertView`), AudioToolbox
- **IDE:** Xcode 4.x with Interface Builder (XIB)

### Build Steps (Historical)

1. Open `Project/iLuck.xcodeproj` in Xcode (originally Xcode 4.x)
2. Select an iOS Simulator target
3. Build and run (`Cmd+R`)

> **Note:** Modern Xcode versions may require adding `-fno-objc-arc` compiler flags since the project uses manual reference counting. Pre-built `.ipa` and `.app` bundles are available in the `Build/` directory.
