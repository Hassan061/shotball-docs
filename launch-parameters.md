# Launch Parameters

ShotBall reads command-line arguments at startup inside `UShotballGameInstance::Init()`. These parameters let you configure server behaviour, networking, game modes, and special runtime modes without recompiling — useful for dedicated server deployments, QA automation, LAN setups, and content-capture sessions.

All parameters follow the standard Unreal Engine `-key=value` convention and are parsed using `FParse::Value` (strings / integers) or `FParse::Bool` (booleans). If a parameter is omitted the game falls back to its Blueprint-defined default.

---

## Parameter Reference

| Parameter | Type | Example | Description |
|---|---|---|---|
| `-region=` | `String` | `-region=EU` | Sets the server region that clients will use when matchmaking or connecting. The value is forwarded to the Blueprint variable via `SetServerRegion`. If omitted, the default region configured in Blueprint is used. |
| `-sgm=` | `String` | `-sgm=ShotBall_GameMode` | Specifies the server Game Mode class name. Passed to `SetServerGameMode` in Blueprint so the server can load the correct game-mode asset at runtime. |
| `-Bport=` | `String` | `-Bport=7788` | Sets the Beacon port — the lightweight UDP port used by the Online Beacon system for pre-matchmaking "heartbeat" checks between clients and the server. |
| `-IsServerCheck=` | `Bool` | `-IsServerCheck=true` | When `true`, puts the game into **server-check mode**: a lightweight headless pass used to verify the server is healthy and reachable before players are routed to it. |
| `-maxplayers=` | `Integer` | `-maxplayers=6` | Overrides the maximum number of players allowed in the session. Must be greater than 0; invalid values are ignored. Blueprint default applies otherwise. |
| `-gamemodetype=` | `String` | `-gamemodetype=Ranked` | A secondary tag that refines the game mode category (e.g. `Ranked`, `Casual`, `Custom`). Passed to `SetServerGameModeType` in Blueprint. Only applied when the parameter is present. |
| `-timer=` | `Integer` | `-timer=120` | Sets the "All-In" round timer (in seconds). Must be greater than 0. Passed to `SetServerTimerIsAllIn` in Blueprint. Useful for custom lobby configurations with non-default time limits. |
| `-simulation=` | `String` | `-simulation=boot` | Activates a named **simulation mode**. When absent (or empty) the game runs normally with no simulation active. Known types: `boot` — simulates only the startup/boot sequence without a live online session; `session` — simulates a full game session, skipping real online subsystem calls. |
| `-lan=` | `Bool` | `-lan=true` | Forces the session into **LAN mode**, bypassing the online subsystem (Steam / EOS) and using direct UDP discovery instead. Useful for local playtests, internal events, or office LAN setups. |
| `-skipshaders=` | `Bool` | `-skipshaders=true` | Tells the game to skip the in-engine shader compilation step on startup. Intended for fast-boot scenarios (e.g. automation, screenshot passes) where shader correctness is not required. |
| `-replay=` | `Bool` | `-replay=true` | Activates **replay mode**, which puts the game into a state appropriate for recording or playing back a match replay. Passed to `SetReplay` in Blueprint. |
| `-film=` | `Bool` | `-film=true` | Enables **film/cinematic mode** — a content-capture state where UI chrome, HUD elements, or gameplay overlays may be suppressed so artists or marketers can record clean footage. |
| `-customgamemodecode=` | `String` | `-customgamemodecode=ABC123` | Passes an arbitrary short code that identifies a custom game mode preset (e.g. a community-created ruleset). Passed to `SetCustomGameModeCode` in Blueprint for lookup against stored custom rule definitions. |
| `-ip=` | `String` | `-ip=192.168.1.100` | Sets the target server IP address. Used by clients or tools that need to connect to a specific dedicated server directly, bypassing matchmaking. |
| `-port=` | `String` | `-port=7777` | Sets the game server port. Works in conjunction with `-ip=` for direct IP connections. |
| `-config=` | `String` | `-config=TournamentConfig` | Loads a named game configuration preset. Allows different server flavours (tournament rules, content test builds, etc.) to be selected at launch without code changes. |

---

## Use Cases

### Dedicated Server Deployment

A typical Linux dedicated server launch line:

```bash
./ShotBallServer-Linux-Shipping \
  -region=EU \
  -sgm=ShotBall_GameMode \
  -gamemodetype=Ranked \
  -maxplayers=6 \
  -Bport=7788 \
  -port=7777 \
  -timer=120 \
  -IsServerCheck=false
```

### LAN / Local Playtest

```bash
ShotBall.exe -lan=true -maxplayers=4 -port=7777
```

Bypasses all online subsystem calls so you can run a full session without a Steam or EOS connection.

### Automated QA / CI Pipeline

```bash
ShotBall.exe -simulation=session -skipshaders=true -region=EU -sgm=ShotBall_GameMode
```

`-simulation=session` skips live network calls; `-skipshaders` cuts shader compilation time so the game boots faster in headless environments.

### Server Health Check

```bash
ShotBall.exe -IsServerCheck=true -ip=10.0.0.5 -port=7777 -Bport=7788
```

Boots the game in a lightweight verification pass that confirms the target server is reachable before routing real players to it.

### Content Capture / Trailer Recording

```bash
ShotBall.exe -film=true -skipshaders=false
```

Enables cinematic mode so HUD and UI overlays are suppressed, giving a clean viewport for trailer or screenshot capture.

### Replay Playback

```bash
ShotBall.exe -replay=true
```

Launches the game in replay-playback mode. The Blueprint layer handles loading the saved replay file.

### Custom Game Mode (Community Ruleset)

```bash
ShotBall.exe -gamemodetype=Custom -customgamemodecode=SHOTBALL_TURBO
```

Loads a specific community or tournament ruleset identified by the short code. The Blueprint looks up the code against stored custom-mode definitions.

---

## Notes

- Parameters are **case-sensitive** where they appear in the source (e.g. `-Bport=` has a capital `B`).
- Boolean parameters accept `true` / `false` or `1` / `0`.
- All string parameters that contain spaces should be quoted: `-region="North America"`.
- Parameters not listed here that Unreal Engine recognises natively (e.g. `-log`, `-nullrhi`, `-dx12`) still work alongside these custom parameters.
- Source: `ShotBallV28/Source/ShotBallV1/ShotballGameInstance.cpp` — `UShotballGameInstance::Init()`
