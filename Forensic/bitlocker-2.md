### TITLE
>Bitlocker-2
### DESCRIPTION
> Jacky has learnt about the importance of strong passwords and made sure to encrypt the BitLocker drive with a very long and complex password. We managed to capture the RAM while this drive was opened however. See if you can break through the encryption!
Download the disk image here and the RAM dump here

### CATEGORY
> Forensic
### HINT
>Try using a volatility plugin
### DIFFICULTY
>medium
### AUTHOR
> Venax
### TOOLS
> [bitlocker plugin](https://github.com/breppo/Volatility-BitLocker)
### FLAG
> picoCTF{B1tl0ck3r_dr1v3_d3crypt3d_9029ae5b}
### SOLVED
Like the BitLocker-1, the BitLocker-2 gives us 2 files the .dd file and the RAM dump file. Now you can't easily try to crack the user password straight from the encrypted disk image, try to use the RAM dump to find something else. The hint gives you some hints related to "volatility" and the plugin of it. After Googling, I found the BitLocker plugin that allows you to extract the Full Volume Encryption Key (FVEK) from memory.

![{6BEF0843-3086-4BAB-B339-9910DE28B5B9}](https://github.com/user-attachments/assets/d212b4cc-0871-4029-b03a-81f61b5e3309)

The FVEK doesn't give you direct access to the user password but you can use that to crack the disk image. Here I use docker Volatility2 because the base one has some issues with the BitLocker plugin. First, install it:
```
$ git clone https://github.com/p0dalirius/docker-volatility2.git
$ cd docker-volatility2
$ make install
```
After installing successfully, enter the volatility2 workspace and install the BitLocker plugin.
```
$ sudo volatility2docker
$ git clone https://github.com/breppo/Volatility-BitLocker.git
$ cp Volatility-BitLocker/bitlocker.py /volatility/volatility/plugins/
```
Done the setup part. Next, you check the image info to identify the Windows profile used in the memory dump. Extract the mem dump and run the command:
```
volatility2 -f memdump.mem imageinfo
```
![{C97282DA-F54D-4FB6-9031-5A1064B7EBE0}](https://github.com/user-attachments/assets/8f0e863b-69b4-4602-8f3e-f9f23e160c63)

You get the image info of __Win10x64_19041__. Next, extract all the FVEK out.
```
$ mkdir fvek/
$ volatility2 -f memdump.mem bitlocker --profile=Win10x64_19041 --dislocker fvek/
```

![{87601EE2-35C8-4A29-95C6-DC35ED76F533}](https://github.com/user-attachments/assets/bbbe9f24-be70-448b-8647-da2cfb1b5d94)


The exact FVEK is the first one, exit the docker and mount the BitLocker with the FVEK. But first, install Dislocker if you haven't. Then mount the BitLocker image to loop0 or whatever loop you have.
```
$ sudo apt install dislocker -y
$ sudo losetup -fP --show bitlocker-2.dd
$ lsblk
sudo dislocker -V /dev/loop0 -k fvek/0x8087865bead0-Dislocker.fvek -- /mnt/bitlocker
sudo mkdir /mnt/decrypted
sudo mount -o loop /mnt/bitlocker/dislocker-file /mnt/decrypted
```

![{8B183809-F4FC-4073-9113-73CA4A27E677}](https://github.com/user-attachments/assets/11d77ec0-77a4-4a3c-8109-32550d25687f)

WHAT??

After several of failure to detect the right file for almost 30 mins. Finally, I realized that you couldn't mount the BitLocker image with the "-o loop". "-o loop" is used when mounting a file (instead of a block device). But the BitLocker image uses NTFS filesystem so you must use "-t ntfs-3g" instead. The correct command is:
```
sudo mount -t ntfs-3g /mnt/bitlocker/dislocker-file /mnt/decrypted
```
So I modified the command and got the flag.


__P/S__: There is a much faster and easier way to solve this challenge.
```
cat memdump.mem | grep 'picoCTF{' --text
```
![{EB87FEE8-9793-4DD6-9D39-CF54F7DE726B}](https://github.com/user-attachments/assets/1ff2f9b6-5219-48d0-89cb-14e744dd3415)

:))

### END!!
