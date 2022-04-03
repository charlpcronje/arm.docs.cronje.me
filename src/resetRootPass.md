# Reset root password

- Power down and pull the SD card out from your Pi and put it into your computer.
- Open the file 'cmdline.txt' and add 'init=/bin/sh' to the end. This will cause - the machine to boot to single user mode.
- Put the SD card back in the Pi and boot.
- When the prompt comes up, type 'su' to log in as root (no password needed).
- Type "passwd pi" and then follow the prompts to enter a new password.
- Shut the machine down, then pull the card again and put the `cmdline.txt` file back the way it was by removing the 'init=/bin/sh' bit.

