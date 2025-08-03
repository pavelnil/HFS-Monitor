# [HFS Monitor](../../releases/)

---

## Project Description

**[HFS Monitor](../../releases/)** is a monitoring tool for **HTTP File Server version 3 (HFS 3)**, providing detailed **real-time network activity analytics**. The application displays traffic graphs, active connections, and current data transfer speeds. **Portable version** requiring no installation. **All settings are stored in a file**. Inherits certain principles from the **HFS 2.3** ideology.
![screenshot](../../blob/main/screenshots/screenshot.jpg)

---

## Key Features

### Real-Time Traffic Monitoring
* **Incoming/outgoing traffic graphs** with zoom and pan capabilities.
<details>
  <summary><i>Typical hotkeys and mouse gestures for interacting with OxyPlot plots:</i></summary>

```
   Mouse Actions

    Pan:  
      Right mouse button
      Alt + Left mouse button

    Zoom:  
      Mouse wheel
        Zoom by rectangle:  
          Ctrl + Right mouse button
          Middle mouse button
          Ctrl + Alt + Left mouse button

    Reset (zoom/pan):  
      Ctrl + Right mouse button double-click
      Middle mouse button double-click
      Ctrl + Alt + Left mouse button double-click

    Show 'tracker' (display coordinates/data at mouse position):  
      Left mouse button
      Shift + Left mouse button (for points-only tracker)
      Ctrl + Left mouse button (for free tracker, showing mouse coordinates)


   Keyboard Actions

    Pan:  
      Up/Down/Left/Right arrow keys
      Ctrl + Arrow key (for fine pan)

    Zoom In:  
      'Add' key (on numeric keypad)
      'PageUp' key
      Ctrl + 'Add' / 'PageUp' (for fine zoom)

    Zoom Out:  
      'Subtract' key (on numeric keypad)
      'PageDown' key
      Ctrl + 'Subtract' / 'PageDown' (for fine zoom)

    Reset axes:  
      'R' key
      'Home' key

    Copy bitmap to clipboard:  
      Ctrl + C

    Copy code (OxyPlot model definition):  
      Ctrl + Alt + C

    Copy properties:  
      Ctrl + Alt + R

You can find more details by searching Google or examining the `PlotController.cs` source code in the OxyPlot repository.
```

</details>

* **Peak speed values** with automatic reset.
* Support for **data updates up to twice per second**.

### Connection Management
* **Active connections table** displaying IP addresses, users, and traffic details.
* **IP blocking** via context menu. Unblocking requires manual deletion of the corresponding entry from the `config.yaml` file in **HFS**.

### HFS Integration
* **Automatic detection of HFS console**. The HFS console must typically be launched in interactive mode and before starting HFS Monitor for successful window detection.
* Options to **hide/show the console** during startup/shutdown.
* Compatibility with the [**hfs-monitor-agent plugin**](../../../hfs-monitor-agent/) for statistics and status collection.

### Flexible Configuration
<details>
  <summary><i><b> Spoiler hfsmonitor.cfg</b></i></summary>

  ```cfg
# Configuration HFS Monitor:
# Port HTTP File Server on localhost
# (default 80)
hfsport=80

# HFS Monitor Main Window Settings:
#
# The HFS Console must be launched before HFS Monitor.
# If not, HFS Monitor might be unable to determine
# the window name required for hiding it.
# (Boolean 0/1)
hide_console_on_startup=1

# (Boolean 0/1)
unhide_console_on_exit=1

# (Boolean 0/1)
start_minimized=0

# Part of the code uses a hardcoded 5-second refresh timer.
# Another part is configured here. This is responsible for
# updating the graph, connections, speed readings, and so on.
# High values minimize resource usage.
# (0.5 - 60 seconds)
update_interval_seconds=0.5

# Speed maximum values reset interval (in minutes).
# Resets "Peak In" and "Peak Out" speed values after this period.
max_reset_minutes=180

# Chart settings:
# Changing settings might break the chart over time or stress CPU.
# Use with caution.
#
# Number of minutes displayed by default on the chart (visible area)
# With auto-scroll enabled, the chart will show the last N minutes.
# High view values with a large number of points impact CPU load.
view_minutes=5

# Maximum data points per series (incoming/outgoing traffic).
# Older points are automatically removed when the limit is exceeded.
# Rendering 100,000 points at a refresh rate of 0.5 seconds can result
# in CPU utilization of up to 25% on a quad-core processor in some cases.
# Drawing 1,200 points for a 10-minute visible area at a 0.5-second
# refresh rate causes less than a 1% CPU load.
max_data_points=22000

# Graph auto-scroll timeout (in seconds).
# If there's no user interaction with the graph for this duration,
# automatic scrolling to the latest data is re-enabled.
auto_scroll_timeout=2

# Minimum graph zoom scale (in seconds).
# Defines the smallest time interval visible when zooming in.
graph_min_zoom_seconds=10

# Maximum graph zoom scale (in hours).
# Defines the largest time interval visible when zooming out.
# Must equal graph_max_past_hours. If not, the chart might collapse.
graph_max_zoom_hours=3

# Maximum history depth (in hours).
# Limits how far back in time you can view on the graph.
# Must equal graph_max_zoom_hours. If not, the chart might collapse.
graph_max_past_hours=3

# Maximum future offset (in minutes).
# Limits how far forward you can shift the graph view.
# This is necessary to prevent collapse in some cases.
graph_max_future_minutes=5

# Themes:
# ClassicTheme/ClassicTheme2/CyberTheme/GrayTheme/HFSTheme/GradientTheme
# DarkTheme/DarkTheme2/DarkTheme3/DarkTheme4/DarkTheme5/GradientBlueTheme
# LightTheme/LightTheme2/LightTheme3/MatrixTheme/NeonTheme/PeachTheme
theme=LightTheme
  ```
</details>

* **Configuration file (`hfsmonitor.cfg`)** for customization:
    * Server port, update intervals.
    * Graph settings (scale, data depth).
    * Startup behavior (minimization, HFS console hiding).
    * **Theme selection** (15+ variants).

![themesheet](../../blob/main/screenshots/themesheet.jpg)

### Background Mode
* Operation in **system tray** with an icon dynamically displaying connection count from **unique IP** addresses.
![iconssheet1](../../blob/main/screenshots/iconssheet1.jpg)
* **Server status notifications**. ![iconssheet2](../../blob/main/screenshots/iconssheet2.jpg)
* **Tray icon tooltip** detailing active sessions.
![tooltiptray](../../blob/main/screenshots/tooltiptray.jpg)

---

## Technical Specifications

* **Architecture**: WPF application on **.NET 6**.
* **Data visualization**: [OxyPlot](https://oxyplot.github.io/) library for graphs.
* **System integration**:
    * **WinAPI** for HFS console window management.
    * [TaskbarIcon](https://github.com/hardcodet/wpf-notifyicon) library for system tray functionality.
* **Plugin security**: Access rights verification for **HFS API** (only `localhost`).
* **Error handling**: Global exception interception and file logging.

---

## Requirements

* **Server**: **HTTP File Server version 3 (HFS 3)** with [**hfs-monitor-agent** plugin](../../../hfs-monitor-agent/) installed.
* **Client**: **.NET 6 Runtime**, Windows 7 or later.

---

## Use Cases

* File server administration.
* Real-time network load analysis.
* Detection of unauthorized connections.
* Server bandwidth optimization.

---
Support:
* BTC: `bc1qeuq7s8w0x7ma59mwd4gtj7e9rjl2g9xqvxdsl6`
* TON: `UQAOQXGtTi_aM1u54aQjb8QiXZkQdaL9MDSky5LHN0F5-yF2`
