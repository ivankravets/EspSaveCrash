# EspSaveCrash [![Build Status](https://travis-ci.org/krzychb/EspSaveCrash.svg?branch=master)](https://travis-ci.org/krzychb/EspSaveCrash)

Save exception details and stack trace anytime and anywhere the ESP8266 crashes. Implement it in your sketch in two simple steps.


## Overview

Do you face random crashes of ESP8266? Is it difficult to capture diagnostic information because module is in some remote location? Or maybe module crashes after several hours of operation and it is inconvenient to keep serial monitor open until it fails?

EspSaveCrash is a handy little library that will keep automatically catching and saving crash information to ESP8266 module's flash in case it fails due to exception or software WDT. You will then be able to analyze the crash log and decode the stack trace using [ESP Exception Decoder](https://github.com/me-no-dev/EspExceptionDecoder).

You will implement it in your sketch in two simple steps.

1. Include the library
  ```cpp
  #include "EspSaveCrash.h"
  ```

2. Print out saved crash details
  ```cpp
  SaveCrash.print();
  ```

Check provided example sketch [SimpleCrash.ino](https://github.com/krzychb/EspSaveCrash/blob/master/examples/SimpleCrash/SimpleCrash.ino) to see how it works. To clear crash history from the flash use `SaveCrash.clear()`.


## Use it With

* Any ESP8266 module programmed using [esp8266 / Arduino](https://github.com/esp8266/Arduino) core.


## Functionality

* Registers callback to automatically capture and save crash details
* Captures several crashes to save them to ESP8266 module's flash
* Captures exceptions and software WDT restarts (not hardware WDT)
* The following information is saved:
  * Time of crash using the ESP's milliseconds counter
  * Reason of restart - see [rst cause](https://github.com/esp8266/Arduino/blob/master/doc/boards.md#rst-cause)
  * Exception cause - see [EXCCAUSE](https://github.com/esp8266/Arduino/blob/master/doc/exception_causes.md#exception-causes-exccause)
  * `epc1`, `epc2`, `epc3`, `excvaddr` and `depc`
  * Stack trace in format you can analyze with [ESP Exception Decoder](https://github.com/me-no-dev/EspExceptionDecoder)
* Automatically arms itself to operate after each restart of power up of module
* Saves crash details within defined flash space and then stops


## Examples

Library comes with [examples](https://github.com/krzychb/EspSaveCrash/tree/master/examples) that let you trigger some exceptions and see how to visualize saved data. 

Example output of `SaveCrash.print()` on a serial terminal is shown below.

```
Crash information recovered from EEPROM
Crash # 1 at 21040 ms
Reason of restart: 2
Exception cause: 0
epc1=0x40107001 epc2=0x00000000 epc3=0x00000000 excvaddr=0x00000000 depc=0x00000000
>>>stack>>>
3ffefbf0: 00000000 00000000 3ffeebd0 40201f58 
3ffefc00: 3fffdad0 00000000 3ffeebec 40202a20 
3ffefc10: feefeffe feefeffe 3ffeec00 40100114 
<<<stack<<<
Crash # 2 at 4322 ms
Reason of restart: 2
Exception cause: 28
epc1=0x40201f83 epc2=0x00000000 epc3=0x00000000 excvaddr=0x00000000 depc=0x00000000
>>>stack>>>
3ffefbf0: 00000000 00000000 3ffeebd0 40201f81 
3ffefc00: 3fffdad0 00000000 3ffeebec 40202a20 
3ffefc10: feefeffe feefeffe 3ffeec00 40100114 
<<<stack<<<
EEPROM space available: 0x0159 bytes
```


## Tested With

### Arduino Core

* [Esp8266 / Arduino](https://github.com/esp8266/Arduino) core [2.3.0](https://github.com/esp8266/Arduino/releases/tag/2.3.0) for Arduino IDE and Visual Micro
* [framework-arduinoespressif](http://platformio.org/platforms/espressif) version 13 for PlatformIO


### Programming Environment

* [Arduino IDE](https://www.arduino.cc/en/Main/Software) 1.6.9 portable version running on Windows 7 x64
* [PlatformIO IDE](http://platformio.org/platformio-ide) 1.3.0 CLI 2.11.0 running on Windows 7 x64
* [Visual Micro](http://www.visualmicro.com/) 1606.17.10 with Visual Studio Community 2015 running on Windows 7 x64


## Contribute

Feel free to contribute to the project in any way you like! 

If you find any issues with code or descriptions please report them using *Issues* tab above. 


## Author

krzychb


## Credits

* Preparation of this library has been inspired by issue [#1152](https://github.com/esp8266/Arduino/issues/1152) in [esp8266 / Arduino](https://github.com/esp8266/Arduino) repository.
* Development was possible thanks to [Ivan Grokhotkov](https://twitter.com/i_grr), who clarified how to register a crash callback and suggested to save crash information on flash. 
* Actual implementation of library has been done thanks to [djoele](https://github.com/djoele) who provided first working code and suggested to convert it into general functionality.


## License

[GNU LESSER GENERAL PUBLIC LICENSE - Version 2.1, February 1999](LICENSE)
