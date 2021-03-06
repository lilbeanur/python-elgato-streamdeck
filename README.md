# Python Elgato Stream Deck Library

![Example Deck](ExampleDeck.jpg)

This is an open source Python 3 library to control an
[Elgato Stream Deck](https://www.elgato.com/en/gaming/stream-deck) directly,
without the official software. This can allow you to create your own custom
front-ends, such as a custom control front-end for home automation software.

[PyPi Project Entry](https://pypi.org/project/streamdeck/)

[Online Documentation](https://python-elgato-streamdeck.readthedocs.io)

## Status:

Working - you can enumerate devices, set the brightness of the panel(s), set
the images shown on each button, and read the current button states.

Currently the following StreamDeck product variants are supported:
* StreamDeck Original (both V1 and V2 hardware variants)
* StreamDeck Mini
* StreamDeck XL

## Package Dependencies:

The library core has no special dependencies, but does use one of (potentially)
several HID backend libraries. You will need to install the dependencies
appropriate to your chosen backend.

The included examples require the PIL fork "Pillow", although it can be swapped
out if desired by the user application for any other image manipulation library.

### HID Backend (recommended)

The recommended HID backend is the aptly named
[HID Python library](https://pypi.org/project/hid/), which should work across
the three major (Windows, Mac, Linux) OSes and is the most up-to-date. This can
be installed via `pip3 install hid`.

Note that Windows systems requires additional manual installation of a DLL in
order to function. The latest source for this DLL is the
[libUSB GitHub project](https://github.com/libusb/hidapi/releases).

On Linux, this HID library can use one of two system libraries for HID
communication - `libhidapi-hidraw0` or `libhidapi-libusb0`. Only the latter will
work, and the former (if present) should be uninstalled.

Despite the additional setup, this will give the best results in terms of
reliability and performance.

### HIDAPI Backend (not recommended)

Another option is the older
[HIDAPI Python library](https://pypi.org/project/hidapi/), which originates from
the same original project as the HID library listed above, but is now entirely
unmaintained. This can be installed via `pip3 install hidapi`.

Several users report issues with this backend due to various bugs
that have not been patched in the packaged library version, but support for it
is included in this library due to its simple one-line setup on all three major
platforms.

Note the library package name conflicts with the HID backend above; only one or
the other should be installed at the same time.

## Package Installation:

Install the library via pip:

```
pip install streamdeck
```

Alternatively, manually clone the project repository:
```
git clone https://github.com/abcminiuser/python-elgato-streamdeck.git
```

## Raspberry Pi Installation:

The following script has been verified working on a Raspberry Pi (Model 2 B)
running a stock Debian Buster image, to install all the required dependencies
needed by this project:

```
# Ensure system is up to date, upgrade all out of date packages
sudo apt update && sudo apt dist-upgrade -y

# Install the pip Python package manager
sudo apt install -y python3-pip

# Install system packages needed for the Python hid package installation
sudo apt install -y libudev-dev libusb-1.0-0-dev

# Install system packages needed for the Python Pillow package installation
sudo apt install -y libjpeg-dev zlib1g-dev

# Install python library dependencies
pip3 install hid
pip3 install pillow

# Add udev rule to allow all users non-root access to Elgato StreamDeck devices:
sudo tee /etc/udev/rules.d/10-streamdeck.rules << EOF
	SUBSYSTEMS=="usb", ATTRS{idVendor}=="0fd9", GROUP="users"
	EOF

# Install the latest version of the StreamDeck library via pip
pip3 install streamdeck

# Alternatively, install git and check out the repository
#sudo apt install -y git
#git clone https://github.com/abcminiuser/python-elgato-streamdeck.git
```

Note that after adding the `udev` rule, a restart will be required in order for
it to take effect and allow access to the StreamDeck device without requiring
root privileges.

## Credits:

I've used the reverse engineering notes from
[this GitHub](https://github.com/Lange/node-elgato-stream-deck/blob/master/NOTES.md)
repository to implement this library. Thanks Alex Van Camp!

Thank you to the following contributors, large and small, for helping with the
development and maintenance of this library:

- [Aetherdyne](https://github.com/Aetherdyne)
- [BS-Tek](https://github.com/BS-Tek)
- [dirkk0](https://github.com/dirkk0)
- [dubstech](https://github.com/dubstech)
- [Kalle-Wirsch](https://github.com/Kalle-Wirsch)
- [pointshader](https://github.com/pointshader)
- [spidererrol](https://github.com/Spidererrol)
- [Subsentient](https://github.com/Subsentient)
- [shanna](https://github.com/shanna)

If you've contributed in some manner, but I've accidentally missed you in the
list above, please let me know.

## License:

Released under the MIT license:

```
Permission to use, copy, modify, and distribute this software
and its documentation for any purpose is hereby granted without
fee, provided that the above copyright notice appear in all
copies and that both that the copyright notice and this
permission notice and warranty disclaimer appear in supporting
documentation, and that the name of the author not be used in
advertising or publicity pertaining to distribution of the
software without specific, written prior permission.

The author disclaims all warranties with regard to this
software, including all implied warranties of merchantability
and fitness.  In no event shall the author be liable for any
special, indirect or consequential damages or any damages
whatsoever resulting from loss of use, data or profits, whether
in an action of contract, negligence or other tortious action,
arising out of or in connection with the use or performance of
this software.
```
