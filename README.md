# SethXR
DIY middleware that turns old hardware into game input, eventually supporting full DIY VR.

SethXR repurposes hardware such as **PlayStation Move controllers, PS3 Eye cameras, phone sensors, and gamepads** into usable input for games.

The project starts with simple, conventional game input and gradually expands toward tracked controllers, head tracking, and eventually a complete DIY VR setup.

## What is SethXR?

The basic idea is to take hardware that was never designed to work together and build the software layer that makes it useful.

For example, the first setup takes a PS Move and PS3 Eye:

```text
PS Move + PS3 Eye -> SethXR -> interpreted camera/input signal -> Game     
```

SethXR handles the tracking, interpretation, and translation between the hardware and the game.

The goal is not to make every game directly support PlayStation Move hardware. Instead, the middleware provides conventional game input where possible, allowing existing games to use the hardware without modification.

## Status

*Pre-alpha, active development, no release yet

The repository is currently a skeleton implementation. Move-reading and tracking code are not working yet.

The first release will be made once S1 works end-to-end: a PS Move's orientation driving camera rotation in an actual game.

## Setups

SethXR uses *setups* to describe different hardware and input configurations.

Setups describe different ways the system can be used, and they are not required upgrade paths. A user can continue using an older setup while another setup is being developed.

### S1 — Monitor Input

The first setup uses:

- One PS Move
- One PS3 Eye camera
- A conventional gamepad for movement
- A normal monitor

The PS Move provides camera/aim input while the gamepad handles conventional movement.

Its camera-control model is inspired by the PlayStation Move implementation in Portal 2: pointing the controller in a direction produces continuous camera rotation rather than simply mapping controller orientation directly to the camera.

S1 is particularly well suited to Source-engine games, but the underlying concept is intended to work with conventional games and input systems more generally. Minecraft and other games are also potential targets.

### Future setups

Later setups may experiment with:

- Phone IMU / head orientation
- Phone + VR Box
- Multiple PS3 Eye cameras
- Two PS Move controllers
- HMD tracking
- HMD-mounted tracking cameras
- Stereo rendering and lens correction
- VR runtimes such as OpenXR

These are experimental directions rather than fixed requirements for the project.

## Platform support

*Development currently targets NixOS/Linux.

The long-term goal is to support multiple platforms, including:

- Linux / NixOS
- Debian and other mainstream Linux distributions
- Windows

Nix is currently used as the development environment because that's what i'm on and i don't feel like working with any other OS at least until i finish with this as i already have started development.

*NixOS though, is not planned to be a runtime requirement for the finished project.

## Requirements

### Current development environment

- Linux / NixOS
- Python 3
- PS Move controller(s)
- PS3 Eye camera
- [PSMoveAPI](https://github.com/thp/psmoveapi) (vendored under `vendor/`)

### Planned

Platform-specific backends and installation methods will be added as the project matures.

The eventual goal is for users to run SethXR without needing Nix or NixOS.

## Setup

Currently, the supported development environment is the Nix development shell:

```bash
nix develop
python middleware/main.py
```

## How it works

The current S1 pipeline is:

```text
PS Move + PS3 Eye -> middleware/move_reader.py -> middleware/camera_mapper.py -> middleware/virtual_gamepad.py -> game reads conventional input
```

The architecture is intended to evolve as additional tracking sources, input methods, and output backends are added.

## Project layout

```text
sethxr/
├── flake.nix
├── vendor/
│   └── psmoveapi/
├── middleware/       # S1: reading input, mapping it, emitting it
├── tracking/         # shared pose pipeline, used from S1 onward
├── rendering/       # later setups: stereo rendering, lens correction
├── config/
│   └── settings.toml
└── notes/
    └── s1-log.md
```

## Development

SethXR is being developed incrementally.

Setup variations may be identified with numbers such as:

```text
S1
S1.1
S1.2
S2
S2.1
S3
```

These numbers describe *hardware/input configurations, not software releases.

Software releases are tracked independently using normal release/version numbers.

## License

This project is licensed under the MIT License.

See [LICENSE](LICENSE) for details.

Third-party dependencies retain their respective licenses.

