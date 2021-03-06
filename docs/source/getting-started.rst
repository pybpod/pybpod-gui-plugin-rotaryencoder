.. module:: pybpodapi
   :synopsis: top-level module

*************************************************
Getting started
*************************************************

Requirements
===================

This library requires the following Python 3 packages to be installed.

- pyserial>=3.1.1
- python-dateutil
- numpy
- sip
- pyqt5
- matplotlib
- pyforms==4.901.2
- https://UmSenhorQualquer@bitbucket.org/fchampalimaud/logging-bootstrap.git



Installation
============

Execute the following command to install the library on an existing python3 environment.

.. code:: bash

    pip install -U pybpod-gui-plugin-rotaryencoder


Use the module in the `pybpod-api <http://pybpod-api.readthedocs.io>`_ library
==============================================================================================

This module can be connected and disconnected from the Pybpod-api library.
For this go to your user_settings.py file and add the following configuration.

.. code:: python

    PYBPOD_API_MODULES = [
        'pybpod_rotaryencoder_module'
    ]

.. note::

    The configuration above is configured by default on the pybpod-api library.
    You only need to do it if you have overwritten the **PYBPOD_API_MODULES** variable in your user_settings.py.


Examples
========

Use the rotary encoder module with the pybpod-api
----------------------------------------------------

.. code:: python

    #...

    bpod = Bpod()

    for m in bpod.modules:
        print( m.name, type(m) )

    rotary_encoder = bpod.modules[0]
    rotary_encoder.set_position_zero()

    bpod.stop()




Access the rotary encoder module directly from the USB port
-------------------------------------------------------------

.. code:: python

    from pybpod_rotaryencoder_module.module_api import RotaryEncoderModule

    m = RotaryEncoderModule('/dev/ttyACM1')

    m.enable_stream()

    #print the first 100 outputs
    count = 0
    while count<100:
        data = m.read_stream()
        if len(data)==0:
            continue
        else:
            count += 1
            print(data)

    m.disable_stream()

    print('set', m.set_position(179))
    m.set_zero_position()

    m.enable_thresholds([True, False, True, True, False, False, True, True])
    print(m.current_position())

    m.close()


Configure the rotary encoder using the GUI
------------------------------------------

.. code:: python

    import pyforms
    from pybpod_rotaryencoder_module.module_gui import RotaryEncoderModuleGUI


    pyforms.start_app( RotaryEncoderModuleGUI, geometry=(0,0,600,500) )


.. image:: /_static/rotary-encoder-module.png
   :scale: 100 %


Add the Rotary Plugin to the PyBpod-GUI
================================================


In the GUI settings add the next configuration to the list of plugins

.. code:: python

    GENERIC_EDITOR_PLUGINS_LIST = [
        ...
        'pybpod_rotaryencoder_module',
    ]

After open the PyBpod-GUI a access the plugin in the Tools menu.

.. image:: /_static/pybpod-gui-menu.png
   :scale: 100 %
