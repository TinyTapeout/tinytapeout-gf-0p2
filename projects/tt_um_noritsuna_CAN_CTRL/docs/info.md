## How it works
This CAN only has a transmit function.  
It is intended only to transmit GPS data.  

## How to test
- Arty Z7
    - This code is running.
- ZYBO Z7
    - This board is transmitting dummy GPS data to Arty Z7.
- Raspberry Pi
    - This is the destination for CAN data.
- Analog Discovery 3
    - This generates the clock.

![Development Environment](images/dev_env.png)

## External hardware
A GPS data receiver and a mechanism to transmit that data to this chip are required.

