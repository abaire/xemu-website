There is preliminary support for Xbox Live Communicator emulation, for voice communication in games that support it. A graphical interface to attach this emulated peripheral is not available yet, however you can easily add it by opening the Monitor (`~` key) and entering in the following command:

```
device_add usb-xblc,port=1.x.2,id=xblc
```

where the `x` should be replaced with one of 3,4,1,2 for player 1,2,3,4 respectively.

To remove the device:

```
device_del xblc
```

## Troubleshooting

* You cannot add a Communicator device if the controller's first expansion slot already has an MMU unit assigned to it. Attempting to do so will print an error like `Error: usb port 1.3.2 (bus usb-bus.1) not found (in use?)`. Simply remove the MMU from the first slot for the controller using the xemu `Input` menu before adding the xblc device. You can add an MMU in the second slot.
* You can use `info usb` to verify that the Communicator device was added.
