### TITLE
>flags are stepic
### DESCRIPTION
> A group of underground hackers might be using this legit site to communicate. Use your forensic techniques to uncover their message
### CATEGORY
> Forensic
### HINT
>In the country that doesn't exist, the flag persists.
### DIFFICULTY
>medium
### AUTHOR
> Ricky
### TOOLS
>[Stepic](https://pypi.org/project/stepic/)
### FLAG
>picoCTF{fl4g_h45_fl4g9a81822b}
### SOLVED
Starting with a website containing a bunch of world flags. At first, I don't know what to do next. Look at the hint, it mentions something about the country that doesn't exist => So try to find the flag of the non-existent country. Take a glance and stop at the sus country flag.
![image](https://github.com/user-attachments/assets/fabf9460-dd62-4616-8cac-a2157b32d870)

Upanzi, Republic The Flag with the country flag of "Infrastructure, Network" is definitely not a real country!! 

So inspect the website with F12 and download the flag image from the image source URL. After getting the image. I do a bunch of normal image inspections (binwalk, exiftool, stegcheck, etc.) => binwalk look sus. I do a little binwalk and find out the image contains an ESP32 image, which is a firmware image or binary file that is flashed onto the ESP32 microcontroller. 

![image](https://github.com/user-attachments/assets/3b7b7361-949b-425e-a0a0-80b26957a6ab)

But when I try to dig deeper in that way nothing sus found. Then I take a glance back at the challenge title, it mentions something about __stepic__. Turn out it is a Python image steganography!! Download it:
```
pip install stepic
```
Then run it:
```
stepic -d -i upz.png
```
and you get the flag.

### END!!
