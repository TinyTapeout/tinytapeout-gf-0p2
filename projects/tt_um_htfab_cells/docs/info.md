## How it works

Instantiates one copy of each standard cell from the GF180mcu 7-track library,
and multiplexes the project i/o pins so that the functional behaviour of
each cell can be verified.

The instantiated cells have a total of 210 output pins, arranged into
27 pages of 8 pins each. Once a certain page is selected, those 8 outputs
are mapped to the project output pins.

## How to test

Select a page using `ui_in[4:0]`.

Set the cell inputs using `ui_in[7:5]` and `uio_in[2:0]`.

The cell outputs for the selected page should appear on `uo_out`.

## External hardware

None
