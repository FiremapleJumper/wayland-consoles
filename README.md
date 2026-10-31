# Wayland Consoles

Experimental Fabric 26.3 mod for Linux with craftable console TVs, nested Wayland emulator screens, and Mario/Link character abilities adapted to Minecraft. Based on [Waylandcraft](https://github.com/EVV1E/waylandcraft).

## Install

Download `wayland-consoles-0.3.0+26.3.jar` from this repository into your profile’s `mods` folder. Requires Minecraft 26.3, Java 25, Fabric Loader 0.20.1+, and Fabric API 0.165.0+26.3. Supplied native binaries target Linux x86_64 with glibc 2.43; older systems need a rebuild.

Emulators, cores, ROMs and BIOS files are not included. Install emulators separately and configure their paths. Use your own local game files.

## Features

- 22 console blocks, craftable TV/controller, local ROM library and controllable emulator windows.
- Imported Mario, adult Link and young Link visuals and animations.
- Punch/sword combos, Mario backflip/jump chain, Link auto-jump, shared ledge grabbing/climbing/dropping.
- Link equipment, magic, masks, ocarina songs, HUD and toggleable unlocked kit.

## Controls

Use `/consoles movement sm64`, `oot`, or `oot_child`; use `/consoles movement off` to restore your player/camera. Hold forward while falling to grab a ledge; jump/forward climbs, crouch drops. Left click attacks. Mario stationary crouch+jump backflips. Link: I opens equipment, Z/X/C use items, V shields, R targets, H readies/throws eligible blocks.

Avatar import needs Python 3 and Super Mario 64 USA or Ocarina of Time USA Rev 2. It searches Downloads and the profile ROM folder.

## Source and documentation

[Complete buildable source ZIP](wayland-consoles-source.zip) includes Java/Rust source, Gradle build, tests, licenses, provenance and detailed character controls. See [installation guide](Wayland-Consoles-Guide.md).

## Status

Experimental singleplayer adaptation, not an entire original-game engine or quest port. Minecraft hitboxes remain; custom CoopDX models and synchronized multiplayer characters/video are not implemented. Some equipment shares visual models. Twelve unit tests and Minecraft client tests cover TV input/rendering, characters, attacks, ledges and selected abilities; not every emulator or ability combination has been tested.

## Credits and license

Waylandcraft by EVV1E; this derivative is GPL-3.0-or-later. The source archive preserves licenses and attribution. Mario import uses libsm64 (upstream CC0), decompilation-based geometry, and local ROM textures/animations. Link meshes, masks and HUD textures are extracted locally. No ROM or extracted character cache is distributed. See UPSTREAM.md and tools/avatar/PROVENANCE.md inside the source archive.

Not an official Minecraft, Nintendo, Sony, Sega or Microsoft product.
