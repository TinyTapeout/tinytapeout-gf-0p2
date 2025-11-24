<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

Dog Battle Game is a VGA-based game engine featuring 4 animated "dogs" (colored boxes) that bounce around the screen with physics simulation.

The design includes:
- **Physics engine**: Friction (0.99x decay per frame), elastic collisions, wall bouncing
- **4 dogs**: Each with individual position, velocity, mass, and color
- **Collision detection**: Hit counters track when dogs collide with each other
- **VGA output**: 640x480 @ 25MHz pixel clock with RGB color and sync signals

The game runs continuously, updating positions once per frame (60 FPS) with realistic physics including momentum conservation and energy loss on collisions.

## How to test

Connect a VGA monitor to the output pins. The game will start automatically on power-up and run continuously.

Output pin mapping:
- Pins 0-1: VGA sync signals (HS, VS)
- Pins 2-3: VGA blue (2 bits)
- Pins 4-5: VGA green (2 bits MSB)
- Pins 6-7: VGA red (2 bits MSB)

## External hardware

VGA monitor with 640x480 resolution support. Connect via standard VGA cable or appropriate PMOD adapter.
