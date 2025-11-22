```
░█▀▄░█░█░▀█▀░█▀▀░█▀▄░█▀▀░█▀█░▀█▀░▀░█▀▀
░█▀▄░░█░░░█░░█▀▀░█▀▄░█▀▀░█▀█░░█░░░░▀▀█
░▀▀░░░▀░░░▀░░▀▀▀░▀▀░░▀▀▀░▀░▀░░▀░░░░▀▀▀
       • ▌ ▄ ·. ▄▄▄ . ▄▄ •  ▄▄▄· 
       ·██ ▐███▪▀▄.▀·▐█ ▀ ▪▐█ ▀█ 
       ▐█ ▌▐▌▐█·▐▀▀▪▄▄█ ▀█▄▄█▀▀█ 
       ██ ██▌▐█▌▐█▄▄▌▐█▄▪▐█▐█ ▪▐▌
       ▀▀  █▪▀▀▀ ▀▀▀ ·▀▀▀▀  ▀  ▀ 
 █████   █████ █████ ███████████  █████████ 
░░███   ░░███ ░░███ ░█░░░███░░░█ ███░░░░░███
 ░███    ░███  ░███ ░   ░███  ░ ░███    ░░░ 
 ░███████████  ░███     ░███    ░░█████████ 
 ░███░░░░░███  ░███     ░███     ░░░░░░░░███
 ░███    ░███  ░███     ░███     ███    ░███
 █████   █████ █████    █████   ░░█████████ 
░░░░░   ░░░░░ ░░░░░    ░░░░░     ░░░░░░░░░  
      ,----,     ,----..        ,---,     ,---, 
    .'   .' \   /   /   \    ,`--.' |  ,`--.' | 
  ,----,'    | /   .     :  /    /  : /    /  : 
  |    :  .  ;.   /   ;.  \:    |.' ':    |.' ' 
  ;    |.'  /.   ;   /  ` ;`----':  |`----':  | 
  `----'/  ; ;   |  ; \ ; |   '   ' ;   '   ' ; 
    /  ;  /  |   :  | ; | '   |   | |   |   | | 
   ;  /  /-, .   |  ' ' ' :   '   : ;   '   : ; 
  /  /  /.`| '   ;  \; /  |   |   | '   |   | ' 
./__;      :  \   \  ',  /    '   : |   '   : | 
|   :    .'    ;   :    /     ;   |.'   ;   |.' 
;   | .'        \   \ .'      '---'     '---'   
`---'            `---`                          
```

Extended Play of some of the 2011's BYTEBEAT greatest hits on-a-chip:

| Pin     | Track | Artist |
| --------| ------------- | ------|
| `out0`  | the 42 melody | people on irc |
| `out1`  | fractal trees | danharaj |
| `out2`  | untitled | droid |
| `out3`  | a tune to share | Niklas_Roy |
| `out7`  | sierpinski harmony | miiro |

## How it works

The main module accept parameters from 4x 4-bit parameters buses and generate PWM audio signal on each output pins according to the following bytebeat formulas:

| Pin     | Formula | Original Params |
| --------| --------| ---------------
| `out0`  | `t*({b,a}&t>>{d,c})` | `a=0xa,b=0x2,c=0xa,d=0x0` |
| `out1`  | `t|t%{b,a}|t%(2+{d,c})` | `a=0xf,b=0xf,c=0xf,d=0xf` |
| `out2`  | `t>>a&b?t>>c:-t>>d` | `a=0x6,b=0x1,c=0x5,d=0x4` |
| `out3`  | `t*(t>>a|t>>b)&{d,c}` | `a=0x9,b=0xd,c=0x0,d=0x1` |
| `out7`  | `t*a&(t>>b)|t*c&(t>>d)` | `a=0x5,b=0x7,c=0x4,d=0xa` |

## How to test

- Connect a speaker to the pin you want to "play".
- Tweak parameters pins using binary rotary switches.

## External hardware

- [Rotaty switches](https://akizukidenshi.com/catalog/g/g102276/) w/ 16 positions.
- Speakers or [TinyTapeout Audio PMOD](https://github.com/MichaelBell/tt-audio-pmod) if you want to hear `out7`.
