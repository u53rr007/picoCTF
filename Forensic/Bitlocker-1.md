### TITLE
>Bitlocker-1
### DESCRIPTION
> Jacky is not very knowledgable about the best security passwords and used a simple password to encrypt their BitLocker drive. See if you can break through the encryption!

> Download the disk image here
### CATEGORY
> Forensic
### HINT
>Hash cracking
### DIFFICULTY
>medium
### AUTHOR
> Venax
### TOOLS
>[Hashcat](https://hashcat.net/hashcat/), [OSFMount](https://www.osforensics.com/tools/mount-disk-images.html) (for Window way), [Dislocker](https://www.kali.org/tools/dislocker/) (for Linux way)
### FLAG
> picoCTF{us3_b3tt3r_p4ssw0rd5_pl5!_3242adb1}
### SOLVED
Firstly, you get the BitLocker .dd file. It is a Disk image file. The hint also mentions something about hash cracking, so try to dump the BitLocker hash and crack it. First, use __bitlocker2john__ to dump all the hash to the hash.txt.
```
bitlocker2john -i bitlocker-1.dd > hash.txt
```
There is a bunch of hash being extracted out, but we focus mainly on "__User password hash__":

![image](https://github.com/user-attachments/assets/b3506be8-c4ad-47c9-998e-19d0f2c5e6e0)

Next, use hashcat to crack the password. I used the standard rockyou.txt dictionary to crack.
```
hashcat -m 22100 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

![image](https://github.com/user-attachments/assets/f10b2fcb-a3d6-4127-8be5-400c46515ded)

WWait for about 1 min and the password cracked. The next part is to mount the disk image on a Linux machine or mount it on Windows and enter the password to get the flag.

__Windows way:__

First, you need to download __OSFMount__ to mount .dd as a virtual drive. Then follow the steps:

+ Step 1: Disk image file select bitlocker-1.dd.
+ Step 2: Mount entire image as virtual disk.
+ Step 3: Read-only drive + Physical disk emulation.
+ Step 4: Mount.
+ Step 5: It prompts for a password so type in the password and get the flag file.
![image](https://github.com/user-attachments/assets/9e2dbe78-5d82-44b4-b597-1ecdf2094f53)

__Linux way:__

First you need to download __dislocker__ to decrypt BitLocker.
```
sudo apt install dislocker
```
Then:
```
$ fdisk -l bitlocker-1.dd
$ OFFSET=$((2021161080 * 512))
$ mkdir /mnt/dislocker /mnt/bitlocker
$ sudo dislocker -V bitlocker-1.dd -o $OFFSET -ujacqueline -- /mnt/dislocker
$ sudo mount -o loop /mnt/dislocker/dislocker-file /mnt/bitlocker
$ cat /mnt/bitlocker/flag.txt
```
![image](https://github.com/user-attachments/assets/64bf02ce-f4b3-4e68-91da-98bd58f470af)

And you get the flag.

### END!!
