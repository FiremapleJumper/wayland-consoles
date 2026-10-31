# Wayland Consoles for Fabric 26.3

An experimental, independent Waylandcraft derivative with craftable console blocks,
a craftable TV, local ROM discovery, emulator launching and keyboard/mouse control.
This is a prototype, not a finished universal game-to-Minecraft converter.

## Install

1. Use Minecraft Java **26.3**, Java **25**, Fabric Loader **0.20.1 or later**,
   and Fabric API **0.165.0+26.3 or later for 26.3**.
2. Put `wayland-consoles-0.3.0+26.3.jar` in your instance's `mods` folder.
3. Use Linux for the TV compositor. The provided native binary is for **Linux x86_64,
   glibc 2.43 or later**, built on the current machine. Build from source for an older
   Linux distribution or ARM64. Install libxkbcommon and the `xkbcli` utility.
   `xwayland-satellite` is optional for X11 applications.
4. Install the emulators and RetroArch cores you want to use separately. ROMs, firmware,
   emulator cores, and SM64 CoopDX are not included. Configure firmware in each emulator.
5. Start Minecraft once to create `config/waylandconsoles/config.json` and the ROM folders.
   Do not install original Waylandcraft alongside this mod.

## Play on a TV

- Craft a TV and a console. Place them within **8 blocks** of each other.
- Right-click the console to open its local ROM library. A console selects the nearest TV;
  a TV without a running game selects the nearest console. Each TV has its own local session.
- Put game files in `config/waylandconsoles/roms/<console>/`, or drag files onto the library
  screen to copy them there. Search filters local filenames; it does not search ROM websites.
- Select a game to start the configured emulator on the mod's private Wayland display.
- Right-click a running TV to open its control screen. Keyboard and mouse events go to the
  emulator. Physical gamepads can be configured directly in the emulator.
- **Esc** leaves the TV control screen. **Alt+Q** toggles hard keyboard capture, allowing Esc
  to reach the game. Alt+Q always releases that capture. Leaving releases held keys/buttons.
- The craftable **Game Controller** opens the nearest TV when used. Sneak-use it on a TV to
  stop that TV. The library's Stop TV button and `/consoles stop` also stop sessions.
- Use **Attach window** if an emulator's launcher or Xwayland prevents automatic PID matching.
  It lists windows connected to the nested compositor, not windows on your desktop.

Sessions stop when their TV or console disappears, on dimension changes, and on disconnect.
ROM imports never replace existing files. For a multi-file disc, copy/drop the CUE or GDI
and its BIN/RAW/WAV companions together, preserving their relative filenames. Nested disc
folders can be copied manually. M3U paths and CUE references are interpreted by the emulator.

All 22 consoles, the TV, and controller have crafting recipes, item/block models and drops.
The Wayland Consoles creative tab contains all items. Acquiring redstone unlocks their recipes.

## Systems and emulator configuration

There are blocks and default launcher profiles for NES, SNES, Nintendo 64, Game Boy,
Game Boy Color, Game Boy Advance, Nintendo DS, Nintendo 3DS, Master System, Mega Drive/Genesis,
Game Gear, Sega CD, Saturn, Dreamcast, PlayStation, PlayStation 2, PSP, GameCube, Wii,
original Xbox, Atari 2600, and SM64 CoopDX.

Those profiles describe how to launch installed software. They are not bundled emulators,
and a profile's presence is not a claim that every game has been tested or is compatible.

Most older systems use RetroArch. The default core directory is `/usr/lib/libretro`.
Change `coreDir` and each `commands` array in `config.json` for your installation.
Dolphin is the default for GameCube/Wii, xemu for original Xbox, PCSX2 for PS2, and Azahar
for 3DS. Every command is an argument array passed directly to a process, without a shell.
`{rom}` becomes the selected file's absolute path; `{coreDir}` becomes your configured core folder.
Run `/consoles reload` after editing the file.

For example, the `commands` object can contain:

```json
"n64": ["retroarch", "-L", "{coreDir}/mupen64plus_next_libretro.so", "{rom}"],
"gamecube": ["dolphin-emu", "--exec", "{rom}"],
"sm64coopdx": ["/absolute/path/to/sm64coopdx"]
```

Programs are started with the nested `WAYLAND_DISPLAY` and Wayland toolkit backends.
If Xwayland satellite is available, the nested X11 display is passed too. Use a direct
executable rather than a launcher that immediately exits. Logs are in
`config/waylandconsoles/logs/`. Up to four sessions run by default, configurable from one to eight.

## Ocarina of Time and SM64 CoopDX

**Ocarina of Time on the TV:** place a supplied `.z64`, `.n64` or `.v64` ROM in `roms/n64/`
and use the N64 console. Any emulation compatibility and controller mapping come from your N64 core.

**SM64 CoopDX on the TV:** install/build CoopDX separately, using its own game assets and setup.
Set `commands.sm64coopdx` to the installed executable. If it needs a particular working folder,
set `workingDirectories.sm64coopdx` to that absolute folder. Use the CoopDX console's Launch button.
Its normal gameplay, camera, models and co-op networking run inside that application.
This mod does not join Minecraft networking to CoopDX networking.

**Imported characters in Minecraft:** open Movement demo from a console library, or use:

- `/consoles movement sm64`: Mario, three-jump chain, crouch+jump while moving for a long jump,
  and tap crouch after starting a jump to ground-pound.
- `/consoles movement oot`: adult Link, tap crouch while moving forward to roll;
  crouch+back+jump for a backflip; crouch+left/right+jump for a side hop.
- `/consoles movement oot_child`: young Link with the same OoT movement controls.
- `/consoles movement off`: restore your Minecraft player, movement, and previous camera.

Version 0.3 adds punch/sword attack animations, Mario's backflip, Link's automatic ledge
jump, and ledge grabbing/climbing/dropping for all three characters. Link's I screen offers
equipment, three item slots, magic, masks and ocarina songs, with a toggle between an
unlocked kit and Minecraft equipment/ammo requirements. The HUD imports the ROM's hearts
and icons. See [character controls and ability mappings](Character-Controls.md).

The first selection imports local textures and animation data, then replaces your visible
third-person player with the game character. Link's geometry is read from the ROM; Mario's
geometry comes from libsm64's renderer, using textures and animations read from your ROM.
Idle, walk/run, jump and crouch poses are animated, with Mario long-jump/ground-pound and
Link roll clips. The camera starts in third person; F5 still changes views.

The importer searches `~/Downloads` and the profile's `roms/n64` folder, including nested
folders up to six levels deep. ROMs stay where they are. This version supports **original
Super Mario 64 USA** and **Ocarina of Time USA Rev 2 (1.2)** in Z64/V64/N64 byte order.
It needs **Python 3**. The supplied Mario importer library is Linux x86_64.
No ROM is downloaded. Private generated assets are cached in
`config/waylandconsoles/avatars/`; import logs are in `config/waylandconsoles/logs/`.

Optional config fields let you select files explicitly:

```json
"avatarRoms": {
  "sm64": "/absolute/path/to/Super Mario 64 (USA).z64",
  "oot": "/absolute/path/to/Ocarina of Time USA Rev 2.z64"
},
"romSearchPaths": ["~/Downloads"],
"avatarPython": "python3"
```

These settings are optional and merge with existing configurations. Use `/consoles reload`
after editing. To reimport after changing a ROM, close Minecraft and remove only the relevant
`.avz` files from the private avatar cache, then select the character again.

The character abilities and imported HUD are **singleplayer only**, visible to the local client. They use Minecraft
collisions, player hitboxes and adapted movement/combat/equipment; they do not run the original game
physics or import levels, quests, NPC/trading progression, exact cameras, or arbitrary
CoopDX/DynOS custom models. First-person hands remain Minecraft hands. Play the actual game on a TV for
its complete game logic and CoopDX networking.

## Limits

- Only the local player sees their TV video. Multiplayer video/audio streaming is not implemented.
  Servers can load the common craftable blocks; each Linux client runs its own compositor.
- Audio is emitted by the external emulator, not positioned in Minecraft's world.
- Keyboard and absolute mouse input are implemented. Relative mouse lock, in-world crosshair
  mouse clicking, remote controllers and touch input are not implemented.
- The 26.3 port uses CPU pixel composition/upload and GPU readback for DMA-BUF surfaces.
  Expect a performance cost, especially with multiple TVs or large emulator windows.
  Surfaces are limited to 4096×4096. Start with a 640×480 or similar emulator window.
- OpenGL/EGL is required for DMA-BUF importing. Without it, only applications able to use
  shared-memory Wayland surfaces can display. Hardware DMA-BUF playback requires further testing.
- Windows and macOS can load the common blocks, but this build ships Linux native support only.
- No ROM downloader, universal ROM-to-Minecraft translation, exact OoT/SM64 character port,
  or synchronized Minecraft/CoopDX multiplayer bridge is included.

## Build and test

Install JDK 25, a Rust toolchain, a C linker, pkg-config and libxkbcommon development files.

```bash
./build.sh
```

This builds the locked Rust compositor in release mode and then runs the Gradle build/tests.
The mod JAR is written to `build/libs`. The wrapper pins Gradle 9.8.0 with a SHA-256 checksum.
`./gradlew test` runs the local-library and input-map unit tests.

For the in-game test, also install wayland-client development files, wayland-protocols and
wayland-scanner, then run:

```bash
./tools/build-probe.sh
./gradlew runClientGameTest
```

The game test creates a temporary world, checks recipe registration, places consoles and
a TV, launches an original Wayland test-pattern application, checks pixel colors/PID matching,
sends keyboard and mouse input, saves screenshots and checks cleanup. The ordinary test uses
no commercial ROMs. Set `CONSOLE_AVATAR_TESTS=1` to additionally import the owner's local ROMs,
render all three characters and check animation data and movement restoration.
See `VALIDATION.md` for results from this build.

`tools/avatar/build-libsm64.sh` rebuilds the Mario importer library from pinned upstream
source snapshot (with the sampler patch already applied). It needs make and a C compiler.
The source snapshot contains the generated geometry sources for an offline build.
See `tools/avatar/PROVENANCE.md`.

Pixel assets can be regenerated with `python3 tools/generate_assets.py` (Pillow required).

Based on [Waylandcraft](https://github.com/EVV1E/waylandcraft). See `UPSTREAM.md` and `LICENSE`
for provenance and the GPL-3.0-or-later terms. This is not an official Minecraft, Nintendo,
Sony, Sega or Microsoft product.
