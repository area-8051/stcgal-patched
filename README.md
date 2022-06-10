Apparently, Grigori lost interest in stcgal beginning of 2021.
The problem is that we can't use stcgal with recent MCU (STC8G & STC8H).

CreativeLau has submitted a pull request solving this issue in July 
2021, but it still hasn't been merged as of June 2022.
Instead, Grigori blacklisted the SCT8G to avoid bricking them. :(

It's a pity since merging CreativeLau's pull request would solve issues
related to STC8G and STC8H, which use the same protocol as the 
STCA8K64D4.

IOSetting created a new utility in December 2021, stc8prog, but it's 
also a pity since it loses a lot of functionalities available in stcgal.

This is why I decided to patch stcgal with CreativeLau's changes and 
make it available here, in the hope it could help other STC fans.

References:

https://github.com/grigorig/stcgal/pull/71
https://github.com/grigorig/stcgal/issues/63
https://github.com/grigorig/stcgal/issues/67
https://github.com/IOsetting/stc8prog

>>> For Linux users
------------------------------------------------------------------------

Linux users who have already installed stcgal can upgrade it by cloning 
this repository and issuing the following commands as root:

cp stcgal-patched/stcgal/*.py /usr/lib/python3.10/site-packages/stcgal/
rm -rf /usr/lib/python3.10/site-packages/stcgal/__pycache__


>>> Below is the comment in CreativeLau's pull request
------------------------------------------------------------------------

The calibrate process of new serial STC8A8K64D4 is different. 
In version 1.6 the MCU frequency could be adjusted up to 4 MHz only.

Have done:

1.  Add 'stc8d' in option -P for STC8A8K64D4.

2.  Add subclass Stc8dProtocol(Stc8Protocol).

3.  Add override methods, choose_range, choose_trim, calibrate and 
    build_options.
    
4.  The download baud up to 460800 (should add the 460800 to the 
    definition of BAUDRATES in serial lib which is always in the 
    local disk)
    
    To use:
    
    Set stc8d for option -P. The frequency could be adjusted from 
    1 to 45 MHz. Pay attention to the unit of frequency is kHz.

To do:

The program_mcu method did not consider that the EEPROM in some serial 
could be adjusted. The EEPROM content could not be downloaded correctly, 
when user set the option program_eeprom_split, and give a EEPROM larger 
than 512 Bytes.

>>> Below is the original content of this README.md
------------------------------------------------------------------------

[![Build Status](https://github.com/grigorig/stcgal/workflows/Python%20package/badge.svg?branch=master)](https://github.com/grigorig/stcgal/actions?query=workflow%3A%22Python+package%22)
[![Coverage Status](https://coveralls.io/repos/github/grigorig/stcgal/badge.svg?branch=master)](https://coveralls.io/github/grigorig/stcgal?branch=master)
[![PyPI version](https://badge.fury.io/py/stcgal.svg)](https://badge.fury.io/py/stcgal)

stcgal - STC MCU ISP flash tool
===============================

stcgal is a command line flash programming tool for [STC MCU Ltd](http://stcmcu.com/).
8051 compatible microcontrollers.

STC microcontrollers have an UART/USB based boot strap loader (BSL). It
utilizes a packet-based protocol to flash the code memory and IAP
memory over a serial link. This is referred to as in-system programming
(ISP).  The BSL is also used to configure various (fuse-like) device
options. Unfortunately, this protocol is not publicly documented and
STC only provide a (crude) Windows GUI application for programming.

stcgal is a full-featured Open Source replacement for STC's Windows
software; it supports a wide range of MCUs, it is very portable and
suitable for automation.

Features
--------

* Support for STC 89/90/10/11/12/15/8 series
* UART and USB BSL support
* Display part info
* Determine operating frequency
* Program flash memory
* Program IAP/EEPROM
* Set device options
* Read unique device ID (STC 10/11/12/15/8)
* Trim RC oscillator frequency (STC 15/8)
* Automatic power-cycling with DTR toggle or a custom shell command
* Automatic UART protocol detection

Quickstart
----------

Install stcgal (might need root/administrator privileges):
    
    pip3 install stcgal

Call stcgal and show usage:

    stcgal -h

Further information
-------------------

[Installation](doc/INSTALL.md)

[How to use stcgal](doc/USAGE.md)

[Frequently Asked Questions](doc/FAQ.md)

[List of tested MCU models](doc/MODELS.md)

License
-------

stcgal is published under the MIT license.
