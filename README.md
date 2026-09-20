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

*Pre-alpha, active development, no release yet.*

The repository is currently a skeleton implementation.

The first release will be made once S1 works end-to-end: a PS Move's orientation driving camera rotation in an actual game.

## Setups

SethXR uses "setups" to describe different hardware and input configurations.

Setups describe different ways the system can be used, and they are not required upgrade paths. A user can continue using an older setup while another setup is being developed.

### S1 — Monitor Input

The first setup uses:

* One PS Move
* One PS3 Eye camera
* A conventional gamepad for movement
* A normal monitor

The PS Move provides camera/aim input while the gamepad handles conventional movement.

Its camera-control model is inspired by the PlayStation Move implementation in Portal 2: pointing the controller in a direction produces continuous camera rotation rather than simply mapping controller orientation directly to the camera.

S1 is particularly well suited to Source-engine games, but the underlying concept is intended to work with conventional games and input systems more generally. Minecraft and other games are also potential targets.

### Future setups

Later setups may experiment with:

* Phone-based head tracking
* Phone + VR Box
* Multiple PS3 Eye cameras
* Two PS Move controllers
* HMD tracking
* Full VR rendering

These are experimental directions rather than fixed requirements for the project.

## How it works

The current S1 pipeline is:

```text
PS Move + PS3 Eye -> middleware/move_reader.py -> middleware/camera_mapper.py -> middleware/virtual_gamepad.py -> game reads conventional input
```

The architecture is intended to evolve as additional tracking sources, input methods, and output backends are added.

## Platform support

*Development currently targets NixOS/Linux.*

The long-term goal is to support multiple platforms, including:

* Linux
* Windows

Nix is currently used as the development environment because that's what I'm on and I've already started development here.

*NixOS is not planned to be a runtime requirement for the finished project.*

## Development

Currently, the supported development environment is the Nix development shell:

```bash
nix develop
python middleware/main.py
```

The project is being developed incrementally, and software releases will use normal release/version numbers independently from the setup names.

## License

This project is licensed under the MIT License.

See LICENSE for details.

Third-party dependencies retain their respective licenses.
