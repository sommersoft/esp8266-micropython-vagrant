# CircuitPython SDK Virtual Machine (Combined)

Vagrant-based virtual machine that can compile CircuitPython on the following microcontroller series:

* ATMEL SAMD21 & SAMD51
* Espressif ESP8266

## Dependencies

Hardware requirements:

*  CPU/OS necessary for the needed software
*  ~8GB of available disk space for the virtual machine

You must have the following software installed:

*  [VirtualBox](https://www.virtualbox.org/)
*  [Vagrant](https://www.vagrantup.com/)

## Usage

Clone this repository and navigate to it in a command terminal, then run the
following command to bring the Vagrant virtual machine up and provision it for
compiling the tools:

    vagrant up

The very first time this command is run it will take some time (~30 minutes) to
download the operating system and setup the CircuitPython build toolchain.

After the virtual machine is brought up and provisioned use the following
command to enter an SSH session on it:

    vagrant ssh

## Finalize
Use the following guides to finish setting up the VMs for each microcontroller:

* [ATMEL SAMD](https://github.com/sommersoft/esp8266-micropython-vagrant/blob/combined/atmel-samd.md)
* [ESP8266](https://github.com/sommersoft/esp8266-micropython-vagrant/blob/combined/esp8266.md)

## Additional Resources

* Adafruit Learn Guide for [Building Firmware on ATMEL SAMD21](https://learn.adafruit.com/micropython-for-samd21/build-firmware)

* Adafruit Learn Guide for [Building Firmware on ESP8266](https://learn.adafruit.com/building-and-running-micropython-on-the-esp8266/overview)

    _The above guides are slightly outdated.  MicroPython terminology may be used
    in place of CircuitPython; the method is the same between them._

* See more [details on Vagrant usage here](https://www.vagrantup.com/docs/getting-started/).

## Mentions
The Adafruit Vagrant setups were originally developed by Tony Dicola (@tdicola).

Special Thanks! to @tannewt, @Dan Halbert, and @jerryn on discord/#circuitpython
for their help in troubleshooting the combination of the separate Vagrants into
one toolchain.
