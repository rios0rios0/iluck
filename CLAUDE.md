# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Status

iLuck is a **discontinued** iOS slot-machine game (last worked on 2012-04-12), preserved as a historical reference. No new features or fixes are expected; work here is archival, read-only exploration. See `README.md` for gameplay and `CONTRIBUTING.md` for the historical build note. `.github/copilot-instructions.md` covers the same ground for GitHub Copilot.

## Architecture

- Single-screen UIKit app. `Project/iLuck/MasterViewController.{h,m}` is the whole game: it acts as the `UIPickerView` delegate/dataSource, owns all state, and implements win detection and audio. `AppDelegate.{h,m}` only creates the window and root controller; `MainWindow.xib` wires the picker and Play button.
- Three picker columns each render `UIImageView`s held in `NSArrayColum1/2/3`, built once in `-viewDidLoad`. The six logos (Apple, Mac, Microsoft, Windows, Google, Android) are loaded by filename via `imageNamed:`.
- `-Play:` picks a random row per column with `random()` (seeded once via `srandom(time(NULL))`), declares a win only when all three rows match, then schedules `-PlaySoundWinner` or `-EnableButton` with `performSelector:withObject:afterDelay:`. There are no timers or blocks — delayed selectors drive all sequencing and button re-enabling.
- Sounds are AudioToolbox system sounds (`Play.wav`, `Winner.wav`) created in `-viewDidLoad` and disposed in `-viewDidUnload`/`-dealloc`.

## Conventions (easy to get wrong)

- **Manual reference counting (MRC) — no ARC.** Balance `retain`/`release`/`autorelease` by hand; do not add `__strong`/`__weak`/`@autoreleasepool` or convert to ARC. Properties are backed by `@synthesize`.
- **Type-prefixed identifiers** are the house style: `int_attempts`, `NSArrayColum1`, `UIButtonGo`, `UIPickerMachine`. Match it.
- Picker columns are reached by **KVC**: both `-viewDidLoad` and `pickerView:viewForRow:forComponent:reusingView:` build and read keys named `NSArrayColum<n>` via `setValue:forKey:` / `valueForKey:`. Renaming those ivars silently breaks the picker.
- Pre-modern Objective-C: no collection literals, no auto-synthesized properties. Keep changes in the 2012 style.

## Build

- No package manager, no CI, no tests. Build is Xcode-only: open `Project/iLuck.xcodeproj`, select an iOS Simulator scheme, Cmd+R.
- On modern Xcode add `-fno-objc-arc` to the target's *Other C Flags*, or the MRC code will not compile.
- Prebuilt `Build/iLuck.app` and `Build/iLuck.ipa` run without compiling.
