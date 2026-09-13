# Q-SYS Plugin – Sonos Player

A Q-SYS Designer plugin that controls **one Sonos player per plugin instance**, directly over UPnP/SOAP on the LAN (port 1400). No cloud account, no Sonos app required.

Drop one instance per room into your design, set the **Player** property to the room name (or IP), and wire the pins.

**By David – Threshold Audio** · [threshold.audio](https://threshold.audio)

## Features

- Transport: play/pause, next, previous, seek (transport bar), shuffle, repeat (off / one / all)
- Volume and mute for the player, plus volume/mute for the whole group it belongs to
- Grouping: *Group with* dropdown (any other room, or standalone). Transport commands are automatically routed to the group coordinator
- Now playing: album art, title, artist, album, elapsed/duration, transport state (TV / Line‑In / radio detected)
- Sonos favorites dropdown
- Play any URI (e.g. `http://server/announcement.mp3`)
- Snapshot / Restore – save source, position, play state and all group volumes, play something else, restore
- Command pin for control scripts / UCIs
- Max volume limit

## Installation

1. Copy `Sonos Player v4.0.qplug` to `%USERPROFILE%\Documents\QSC\Q-Sys Designer\Plugins\`
2. Restart Q-SYS Designer. The plugin appears under **Threshold Audio › Sonos Player**.

## Properties

| Property | Description |
|---|---|
| **Player** | Room name (e.g. `Kitchen`) or IP address. Room names are resolved via the Sonos ZoneGroupTopology service; the subnet of the first network interface (or of *Discover from IP*) is scanned to find the first player |
| **Artwork** | On/Off – album art is embedded as base64 in the panel; turn off on slow Designer machines |
| **Poll Rate** | 1 / 2 / 5 s |

## Pins

**Input / Both:** Play, Previous, Next, Shuffle, Repeat, Volume, Mute, Group_Volume, Group_Mute, Time, Group_With (text), Favorite (text), Play_URI, Command, Snapshot, Restore, Standalone, Discover

**Output:** Online, Room, State, Track, Artist, Album, Elapsed, Duration, Repeat_Mode, Coordinator, Status, Command_Result, IP

## Command pin

```
play | pause | stop | next | prev
vol N | groupvol N | mute on/off/toggle | groupmute on/off/toggle
shuffle on/off/toggle | repeat off/one/all/toggle
group ROOM | standalone
favorite NAME or N | uri URL
snapshot | restore | discover
```

The pin is cleared after each command so the same command can be sent again from a wired text control.

## How it works

- Player/group state is read with `ZoneGroupTopology#GetZoneGroupState` (one request to any player on the LAN returns every room, IP, uuid and group).
- Transport and position are polled from the group coordinator, volume/mute from the player itself; topology, play mode and group volumes every 5 s, favorites every 60 s.
- Grouping uses `SetAVTransportURI x-rincon:<coordinator uuid>` / `BecomeCoordinatorOfStandaloneGroup`.

## History

Based on ideas from the "Sonos-Stand Alone" community plugin (v2). v3 was a multi‑zone rewrite; v4 splits into one instance per player for simpler pins and a faster Designer UI.

## License

MIT
