<!-- title -->

# Augment Video

<!-- /title -->

<!-- short-description -->

**A two-channel video player with serial controls implementing Greg's ITP Project in openFrameworks.**

<!-- /short-description -->

## Overview

Augment plays two videos at once in a single window, positioned for a setup spanning two projectors. A keyboard shortcut switches between two pairs of clips, and four more shortcuts send individual characters to a serial device.

The app is written in C++ with [openFrameworks](https://openframeworks.cc/). The source and Xcode project date from December 2010.

## Getting started

### Dependencies

- A Mac with an Xcode toolchain compatible with the original project.
- A legacy openFrameworks distribution with its macOS libraries. The exact version is not recorded. The app uses the older `loadMovie()` and `idleMovie()` APIs, GLUT, and QuickTime; the Xcode project's universal build targets PowerPC and 32-bit Intel.
- Four video files, listed below. The media is not included in this repository.
- A serial device for the serial controls, configured for 115200 baud.

The checked-in project requires a legacy build environment or changes to support a newer toolchain.

### Installation

1. Clone this repository into an openFrameworks `apps/myApps` directory so the project's `../../../libs` paths resolve to the distribution's libraries:

   ```sh
   cd openFrameworks/apps/myApps
   git clone https://github.com/kitschpatrol/augment-video.git
   ```

2. Create `bin/data` inside the repository and add the four movies with these names:

   | Pair | UCLA video        | Augment video                          |
   | ---- | ----------------- | -------------------------------------- |
   | 1    | `ucla1_small.mov` | `over_the_shoulder_computer_1_new.mov` |
   | 2    | `ucla2_new.mov`   | `over_the_shoulder_computer_2_new.mov` |

   To use different filenames, edit the `loadMovie()` calls in [`src/augment.cpp`](src/augment.cpp).

3. Update the serial device path in `augment::setup()` in [`src/augment.cpp`](src/augment.cpp) to match your hardware. The current configuration is:

   ```cpp
   serial.setup("/dev/tty.usbserial-A900aeQz", 115200);
   ```

4. Open [`augment.xcodeproj`](augment.xcodeproj), then build and run the `augment` target. The build output goes into `bin`.

## Usage

The first pair of movies starts playing when the app launches.

| Key          | Action                                        |
| ------------ | --------------------------------------------- |
| <kbd>c</kbd> | Stop the current pair and play the next pair. |
| <kbd>f</kbd> | Toggle fullscreen.                            |
| <kbd>l</kbd> | Send the byte `L` over serial.                |
| <kbd>o</kbd> | Send the byte `O` over serial.                |
| <kbd>g</kbd> | Send the byte `G` over serial.                |
| <kbd>a</kbd> | Send the byte `A` over serial.                |

Switching movies also advances a character in the `LOGA` sequence and logs it. It does not send that character over serial; serial writes happen only through the four letter shortcuts.

### Display layout

The window opens at 2040 × 800 pixels, with both videos drawn at 320 × 263 pixels. The UCLA video starts at `(1200, 0)` and the Augment video at `(0, 500)`.

Change the window dimensions in [`src/main.cpp`](src/main.cpp) and the video positions and sizes in `augment::draw()` in [`src/augment.cpp`](src/augment.cpp) to suit your display arrangement.

The original setup notes call for enabling GLUT's “use extended desktop” option in the app's preferences when spanning projectors.
