# Copilot Instructions for iLuck

## Project Overview

iLuck is a discontinued iOS slot machine game originally developed in **April 2012**. Players tap a **Play** button to spin a three-column `UIPickerView`, each column showing one of six tech company logos (Apple, Mac, Microsoft, Windows, Google, Android). The game detects a win when all three columns land on the same logo and plays audio feedback for both spins and wins.

**Development was discontinued on 2012-04-12.** The repository is preserved as a historical reference. No new features or bug fixes are expected; the primary use case is archival and retrospective exploration.

## Repository Structure

```
iluck/
├── .github/
│   └── copilot-instructions.md   # This file
├── Build/
│   ├── iLuck.app/                # Pre-built application bundle (binary + assets)
│   └── iLuck.ipa                 # Packaged iOS application (sideloadable)
├── Imgs/
│   ├── Backgrounds/              # Background and splash screen PSD/PNG sources
│   ├── Buttons/                  # Button state images (normal / selected)
│   ├── Icons/                    # App icons in multiple sizes + PSD sources
│   └── PickerView/               # Tech company logo PNGs used in the slot columns
├── Project/
│   ├── iLuck.xcodeproj/          # Xcode project file
│   └── iLuck/
│       ├── AppDelegate.{h,m}     # App entry point and window setup
│       ├── MasterViewController.{h,m}  # Core game logic (picker, win detection, audio)
│       ├── MainWindow.xib        # Interface Builder layout (picker + play button)
│       ├── main.m                # UIApplicationMain entry
│       ├── Play.wav              # Spin sound effect
│       ├── Winner.wav            # Win sound effect
│       └── *.png                 # Runtime image assets copied into the bundle
├── CONTRIBUTING.md               # Historical build notes
├── LICENSE
└── README.md
```

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Language | Objective-C (manual reference counting / MRC — **no ARC**) |
| Platform | iOS (iPhone), targeting iOS SDK 5.x |
| UI Framework | UIKit (`UIPickerView`, `UIButton`, `UIActionSheet`, `UIAlertView`) |
| Audio | AudioToolbox (`AudioServicesCreateSystemSoundID`, `AudioServicesPlaySystemSound`) |
| IDE / Build | Xcode 4.x with Interface Builder (XIB files) |
| RNG | `srandom(time(NULL))` + `random()` |

## Architecture and Design Patterns

- **MVC** — standard iOS MVC: `AppDelegate` bootstraps the window; `MasterViewController` owns all game state, UI outlets, and delegate callbacks.
- **Single-screen app** — one `UIViewController` subclass (`MasterViewController`) handles everything.
- **Manual Reference Counting (MRC)** — `retain`/`release`/`autorelease` used throughout; no `@autoreleasepool` blocks. Modern Xcode requires the `-fno-objc-arc` compiler flag for this target.
- **Delegate-based UI** — `UIPickerViewDelegate` and `UIPickerViewDataSource` are both implemented on `MasterViewController`.
- **Delayed selectors** — `performSelector:withObject:afterDelay:` drives button re-enable and sound sequencing instead of timers or blocks.

## Key Source Files

### `Project/iLuck/MasterViewController.{h,m}`

The entire game loop lives here:

- `-Play:` — increments the attempt counter, disables the button, plays the spin sound, randomly selects a row for each of the three picker components, checks for a three-of-a-kind win, and schedules either `-PlaySoundWinner` or `-EnableButton` via `performSelector:afterDelay:`.
- `-PlaySoundWinner` — plays the winner sound, shows a `UIAlertView` with the attempt count, then schedules `-EnableButton`.
- `-EnableButton` — re-enables the Play button.
- `pickerView:viewForRow:forComponent:reusingView:` — returns a `UIImageView` populated from the column's `NSArray` of images (each column shares the same image set).
- `-More:` / `actionSheet:didDismissWithButtonIndex:` — "More Apps" / "About" action sheet.

### `Project/iLuck/AppDelegate.{h,m}`

Standard iOS 5 app delegate — creates the `UIWindow` and sets the root view controller.

## Build Instructions (Historical)

> **Note:** This project no longer builds with modern Xcode out of the box. The steps below reflect the original development environment.

1. Install **Xcode 4.x** (or a modern Xcode with legacy SDK support).
2. Open `Project/iLuck.xcodeproj`.
3. Set the active scheme to an **iOS Simulator** target.
4. If using a modern Xcode, add `-fno-objc-arc` to *Build Settings → Other C Flags* for the `iLuck` target to suppress ARC migration warnings/errors.
5. Press **Cmd+R** to build and run.

A pre-built `.ipa` and `.app` bundle are available in `Build/` and can be used directly without compiling.

## CI/CD

There is no CI/CD pipeline configured for this repository. The project is archived and no automated builds or tests exist.

## Development Workflow

Since the project is archived, typical workflow is read-only exploration. If modifications are needed for historical research:

1. Open `Project/iLuck.xcodeproj` in Xcode.
2. Make source changes in `Project/iLuck/`.
3. Build with **Cmd+B**; run with **Cmd+R** against an iOS Simulator.
4. There are no unit tests — all verification is manual via the Simulator.

## Coding Conventions

- **MRC everywhere** — always balance `retain`/`release`; no `__strong`, `__weak`, or `@autoreleasepool`.
- **IBOutlet / IBAction** — UI connections are declared with `IBOutlet` and `IBAction` keywords and wired in Interface Builder (XIB).
- **`@synthesize`** — property accessors are generated with `@synthesize` (pre-`@property` auto-synthesis era).
- **Naming** — variables are prefixed with their type abbreviation (e.g., `int_attempts`, `NSArrayColum1`, `UIButtonGo`) — this is the existing convention in the codebase.
- **No ARC, no blocks, no modern Objective-C literals** — the codebase predates these features; keep changes consistent with the 2012 style.

## Common Tasks

| Task | How |
|------|-----|
| Run the app | Open `Project/iLuck.xcodeproj` → select a Simulator → Cmd+R |
| Inspect compiled binary | `Build/iLuck.app/iLuck` (ARM slice) |
| Install on a device (historical) | Sideload `Build/iLuck.ipa` with Xcode Organizer or a third-party tool |
| Change picker images | Replace PNGs in `Project/iLuck/` (and `Imgs/PickerView/` for source assets) |
| Add a new tech logo | Add a PNG, add it to each column's `NSArray` in `-viewDidLoad`, update win-detection logic |

## Troubleshooting

- **"ARC forbids explicit message send of 'release'"** — Add `-fno-objc-arc` to the compiler flags for the target (the project uses MRC intentionally).
- **Picker shows blank rows** — Verify that all image files are included in the *Copy Bundle Resources* build phase.
- **No sound on Simulator** — AudioToolbox system sounds are muted on some Simulator configurations; test on a physical device or check the Simulator's audio settings.
- **Build fails on modern Xcode** — The project was created with Xcode 4; open the project and let Xcode migrate the project format, then address any deprecation errors (most `UIAlertView`/`UIActionSheet` APIs were deprecated in iOS 8).
