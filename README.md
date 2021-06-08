# OBS RTC Timecode Generator for Text Sources 

Generate timecode (HH:MM:SS:FF) for a text source based on the computers RTC (Real-Time Clock)

![RTC TCG Script Logo](docs/logo-large.png?raw=true)

**Tips:**

* Synchronize your computer's clock with an internet time server.
* Enable "Update when not in program" only if needed. Otherwise, keep it disabled to lower the CPU load
* A [monospaced / fixed-width font](https://en.wikipedia.org/wiki/Monospaced_font) is recommended to prevent resizing of the timecode every frame
* Remember that Enabling "Show Frames" will require more processing power because every frame must be rendered.

## Why is this useful?

![Timecode Demo](docs/tc-animation.gif?raw=true)

The script was written to create accurate timecode which is based on the computer's time of day.  It can be used for creating slates, time labels on security footage, live cameras, etc. It is useful for checking latency with other live sources and encoders.

## Settings

![Settings Screenshot](docs/settings.png?raw=true)

* **Text Source**: This is the source to which the timecode text will be applied.
* **Mode**: Select the clock mode of either 24 Hour, 12 Hour, or 12 Hour with AM/PM displayed.
* **Show Frames**: Checking this will display the frames component (:FF) of the timecode. This could potentially slow down older computers because every frame must be rendered. You can keep an eye on this with the CPU usage box at the bottom right corner of the OBS application.
* **Prefix Text**: This text will be applied *in front of the timecode*. You may want to add a space at the end, if needed, to add padding to the text.
* **Suffix Text**: This text will be applied *at the end of the timecode*.  You may want to add a space at the end, if needed, to add padding to the text.
* **Update when not in program**: When checked, the time will continue to be updated even when not in program. This can be useful when making isolated recordings for later logging, keeping timecode running on the OBS multiviewer, sending via NDI feeds, ect. By default it is disabled to minimize CPU load, especially with "*show frames*" checked.

## Technical Background

[SMPTE timecode](https://en.wikipedia.org/wiki/SMPTE_timecode) is a standard to label video frames. It is used in television and film productions for synchronization and logging. This program will use your computer's [real-time clock (RTC)](https://en.wikipedia.org/wiki/Real-time_clock) to generate this timecode. Currently, only non-drop frame (NDF) timecode is implemented, even if using a drop frame format such as 29.97 fps. Please note that "drop frame" is a labeling method of skipping certain numbers to prevent clock drift at fractional frame rates (30fps vs 29.97fps), NOT dropping actual frames.

The Lua `os.date` function only reliably returns seconds.  To get frame accuracy, the frame number is incremented every frame, and then reset at the clock's ***second*** change.  This works well and labels every frame regardless of frame rate base.

## Installation / Getting Started

1. [Download](https://github.com/spessoni/obs-timecode-text/releases) the latest version of  ***timecode-text.lua***
3. Create a text source which will display the timecode.
2. Add script via *Tools > Scripts* menu.
3. Configure the script settings.
4. Make sure the source is in program or enable "Update when not in program"

## Troubleshooting

* **Timecode does not advance:** Make sure the source is in program and currently active or enable "Update when not in program". Also make sure the correct source is set and hasn't been renamed.
* **Clock is not correct:** Make sure your computer's clock is displaying correctly, including time zone and daylight savings / standard time. You may also want to synchronize your computer's clock with an internet time server.
* **My CPU usage goes up when I select frames:** Unfortunately this is unavoidable. Each frame is required to be processed and rendered. On most machines, this will only add 1-2% max to the CPU processing load.
