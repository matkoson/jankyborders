# Matkoson JankyBorders

Fork of [FelixKratz/JankyBorders](https://github.com/FelixKratz/JankyBorders) for the Matkoson fleet.

## Why this fork exists

Upstream 1.9.0 leaves overlay windows on screen when `EVENT_WINDOW_DESTROY` arrives with a different space id than the border recorded. `windows_window_destroy` used to require `border->sid == sid`. AeroSpace workspace moves trip that. This fork:

- destroys the border whenever the target window is gone
- sweeps orphans every 5s via `SLSGetWindowOwner`
- recreates the `CGContext` after `SLSSetWindowShape` so resize does not leak surfaces
- marks `is_destroyed` so async update/move paths do not touch a freed border
- drains every tracked overlay on SIGTERM/SIGINT before the process exits (`matkoson-junkyborders stop|cleanup`)

## CLI

`matkoson-junkyborders` on `PATH` (`~/.local/bin`):

```
matkoson-junkyborders help
matkoson-junkyborders version
matkoson-junkyborders status
matkoson-junkyborders start
matkoson-junkyborders stop
matkoson-junkyborders cleanup
matkoson-junkyborders engine
```

The engine is Developer ID signed (`Mateusz Koson`, team `73YQ858MMF`) at `~/.local/libexec/matkoson-junkyborders/borders`.

## Install

```
local/install-matkoson-jankyborders
```

That builds `bin/borders`, signs it, installs the CLI, and bootstraps LaunchAgent `com.matkoson.jankyborders`.

Do not use Homebrew `borders` on this machine once the fork is installed.

## TestFlight

This repo has never shipped TestFlight. Developer ID + LaunchAgent is the ship path unless an ASC app record is created later.
