This documents the decisions I made during the development the e6systems and the reasoning behind them.

# ISO9660
E6os does not support ISO9660 because CD/DVD appears to be dying out and seems to be used less and less. For both booting and transferring files, a modern alternative is using a usb flash stick (or booting from a network).

To boot from a usb stick, it should be formatted with a GPT partitioning scheme and a FAT32 EFI System Partition should be created. For ordinary file transfers, Format the usb stick as FAT32.
