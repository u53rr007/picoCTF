### TITLE
>RED
### DESCRIPTION
> RED, RED, RED, RED. Download the image: red.png

### CATEGORY
> Forensic
### HINT
> The picture seems pure, but is it though?

> Red?Ged?Bed?Aed?

> Check whatever Facebook is called now.
### DIFFICULTY
>easy
### TOOLS
> [aperitsolve](https://www.aperisolve.com/), zsteg, [cyberchef](https://cyberchef.org/)
### AUTHOR
> Shuailin Pan (LeConjuror)
### FLAG
> picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}
### SOLVED
After downloading any images the first thing to check is to open the image to see if it can be opened. The image is just a red square nothing special. After that, try to extract the metadata of an image using Exiftool:
```
exiftool red.png
```
You will see the poem tag in the metadata. Apart from that everything else seems normal for an image.
![image](https://github.com/user-attachments/assets/4215c25a-102b-4a8d-ad5d-68b97f4f8ef1)

After that, I did a few normal checks for hex, compress files (Binwalk), pngcheck, etc. And stopped at the zsteg check. When I checked for LSB using zsteg there are some sussy __base64__ strings.
```
zsteg --lsb red.png
```

![image](https://github.com/user-attachments/assets/f87e53b6-4c94-41a0-b1eb-78b08591692d)

Take that string and decode it and you get the flag.

P/S: You can use Aperitsolve for a faster way to find the LSB.

![image](https://github.com/user-attachments/assets/7011b1af-b86b-4357-bea9-8e6fc2f737e6)

### END!!
