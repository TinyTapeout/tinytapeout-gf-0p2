<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

Manually-instantiated gf180mcuD inverter cells form a chain out of chain segments of varying lengths, allowing the user to select given points in the overall chain to loop back to produce a ring oscillator. This makes a configurable ring oscillator that is expected to be able to oscillate from about 15MHz up to 350MHz.

This (or an external clock) then can be selected to drive a "worker" module: a counter which counts up to 3000.

Alongside this is a VGA sync generator which takes its pixel colour from whatever is in the upper 6 bits of the worker's counter at the time. The worker is reset during HBLANK of each VGA line.

It's expected that at the faster ring oscillator speeds, the counter will reach its target of 3000 sooner than the width of the VGA line but with some jitter... or the counter/compare logic will break down because it's too fast.


## How to test

Set `clksel[3:0]` to (say) 10, or anything greater than 1.

Set `mode[1:0]` to 0 (though these are unused at the time of writing; TBA).

Set `vga_mode` to 0.

Attach a Tiny VGA PMOD to `uo_out`.

Supply a 25MHz clock to the system `clk`, and assert reset for at least 2 clocks.

Expect to see vertical coloured bars on screen, but expect some jitter. Their width should increase as you increase `clksel`.

Measure the ring oscillator on `uio_out[7:4]` -- `uio_out[4]` is the raw oscillator output, and the higher bits are it divided by powers of 2.


## External hardware

Tiny VGA PMOD and a VGA monitor.

