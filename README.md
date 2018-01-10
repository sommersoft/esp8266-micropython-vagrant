
# CircuitPython SDK Virtual Machine (Combined)

Vagrant file to build a virtual machine that can compile CircuitPython on the following microcontroller series:

* ATMEL SAMD21 & SAMD51
* Espressif ESP8266

# Dependencies

You must have the following software installed:

*  [VirtualBox](https://www.virtualbox.org/)
*  [Vagrant](https://www.vagrantup.com/)

# Usage

Clone this repository and navigate to it in a command terminal, then run the
following command to bring the Vagrant virtual machine up and provision it for
compiling the tools:

    vagrant up

After the virtual machine is brought up and provisioned use the following
command to enter an SSH session on it:

    vagrant ssh

# Finalize
Use the following guides to finish setting up the VMs for each microcontroller:

* [ATMEL SAMD](https://github.com/sommersoft/esp8266-micropython-vagrant/blob/combined/atmel-samd.md)
* [ESP8266](https://github.com/sommersoft/esp8266-micropython-vagrant/blob/combined/esp8266.md)

# Mentions
The Adafruit Vagrant setups were originally developed by Tony Dicola (@tdicola).
Special Thanks! to @Dan Halbert and @jerryn on discord/#circuitpython for their help
in troubleshooting the combination of the separate Vagrants into one toolchain.
