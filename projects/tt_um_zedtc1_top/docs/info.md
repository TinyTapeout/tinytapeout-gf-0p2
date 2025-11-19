## How it works

See README.md

## How to test

Use e.g TTL-232 to UART interface such that bytes sent via UART to `UART_RX` and observe SPI transfers on
`SPI_SCL`, `SPI_CS`, and `SPI_MOSI`.
Data returned from SPI is echo via `UART_TX`.

