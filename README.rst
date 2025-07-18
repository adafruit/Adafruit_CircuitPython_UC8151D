Introduction
============


.. image:: https://readthedocs.org/projects/adafruit-circuitpython-uc8151d/badge/?version=latest
    :target: https://docs.circuitpython.org/projects/uc8151d/en/latest/
    :alt: Documentation Status


.. image:: https://raw.githubusercontent.com/adafruit/Adafruit_CircuitPython_Bundle/main/badges/adafruit_discord.svg
    :target: https://adafru.it/discord
    :alt: Discord


.. image:: https://github.com/adafruit/Adafruit_CircuitPython_UC8151D/workflows/Build%20CI/badge.svg
    :target: https://github.com/adafruit/Adafruit_CircuitPython_UC8151D/actions
    :alt: Build Status


.. image:: https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json
    :target: https://github.com/astral-sh/ruff
    :alt: Code Style: Ruff

CircuitPython `displayio` driver for US8151D-based ePaper displays


Dependencies
=============
This driver depends on:

* `Adafruit CircuitPython <https://github.com/adafruit/circuitpython>`_

Please ensure all dependencies are available on the CircuitPython filesystem.
This is easily achieved by downloading
`the Adafruit library and driver bundle <https://circuitpython.org/libraries>`_
or individual libraries can be installed using
`circup <https://github.com/adafruit/circup>`_.

Adafruit 2.9" Flexible 296x128 Monochrome eInk / ePaper Display

`Purchase one from the Adafruit shop <http://www.adafruit.com/products/4262>`_


Installing from PyPI
=====================

On supported GNU/Linux systems like the Raspberry Pi, you can install the driver locally `from
PyPI <https://pypi.org/project/adafruit-circuitpython-uc8151d/>`_.
To install for current user:

.. code-block:: shell

    pip3 install adafruit-circuitpython-uc8151d

To install system-wide (this may be required in some cases):

.. code-block:: shell

    sudo pip3 install adafruit-circuitpython-uc8151d

To install in a virtual environment in your current project:

.. code-block:: shell

    mkdir project-name && cd project-name
    python3 -m venv .venv
    source .venv/bin/activate
    pip3 install adafruit-circuitpython-uc8151d



Installing to a Connected CircuitPython Device with Circup
==========================================================

Make sure that you have ``circup`` installed in your Python environment.
Install it with the following command if necessary:

.. code-block:: shell

    pip3 install circup

With ``circup`` installed and your CircuitPython device connected use the
following command to install:

.. code-block:: shell

    circup install adafruit_uc8151d

Or the following command to update an existing version:

.. code-block:: shell

    circup update

Usage Example
=============

.. code-block:: python

    import time
    import board
    import displayio
    import fourwire
    import adafruit_uc8151d

    displayio.release_displays()

    # This pinout works on a Feather M4 and may need to be altered for other boards.
    spi = board.SPI()  # Uses SCK and MOSI
    epd_cs = board.D9
    epd_dc = board.D10
    epd_reset = board.D5
    epd_busy = None

    display_bus = fourwire.FourWire(
        spi, command=epd_dc, chip_select=epd_cs, reset=epd_reset, baudrate=1000000
    )
    time.sleep(1)

    display = adafruit_uc8151d.UC8151D(
        display_bus, width=296, height=128, rotation=90, busy_pin=epd_busy
    )

    g = displayio.Group()

    pic = displayio.OnDiskBitmap("/display-ruler.bmp")
    t = displayio.TileGrid(pic, pixel_shader=pic.pixel_shader)
    g.append(t)

    # Place the display group on the screen
    display.root_group = g

    # Refresh the display to have it actually show the image
    # NOTE: Do not refresh eInk displays sooner than 180 seconds
    display.refresh()
    print("refreshed")

    time.sleep(180)



Documentation
=============

API documentation for this library can be found on `Read the Docs <https://docs.circuitpython.org/projects/uc8151d/en/latest/>`_.

For information on building library documentation, please check out `this guide <https://learn.adafruit.com/creating-and-sharing-a-circuitpython-library/sharing-our-docs-on-readthedocs#sphinx-5-1>`_.

Contributing
============

Contributions are welcome! Please read our `Code of Conduct
<https://github.com/adafruit/Adafruit_CircuitPython_UC8151D/blob/HEAD/CODE_OF_CONDUCT.md>`_
before contributing to help this project stay welcoming.
