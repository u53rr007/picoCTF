### TITLE
>Blast from the past
### DESCRIPTION
> The judge for these pictures is a real fan of antiques. Can you age this photo to the specifications?
Set the timestamps on this picture to 1970:01:01 00:00:00.001+00:00 with as much precision as possible for each timestamp. In this example, +00:00 is a timezone adjustment. Any timezone is acceptable as long as the time is equivalent. As an example, this timestamp is acceptable as well: 1969:12:31 19:00:00.001-05:00. For timestamps without a timezone adjustment, put them in GMT time (+00:00). The checker program provides the timestamp needed for each.
Use this picture.

### CATEGORY
> Forensic
### HINT
> Exiftool is really good at reading metadata, but you might want to use something else to modify it.
### DIFFICULTY
>medium
### AUTHOR
> syreal
### TOOLS
> [Hexed](https://hexed.it/), [exiftool](https://exiftool.org/), [ghex](https://wiki.gnome.org/Apps/Ghex), [Cyberchef](https://cyberchef.org/)
### FLAG
> picoCTF{71m3_7r4v311ng_p1c7ur3_a4f2b526}
### SOLVED
Firstly, you get the image, original.jpg. The description told us to modify all the timestamps of the image to "1970:01:01 00:00:00.001+00:00," the first day of UNIX. So, open the image using exiftool to see which tag needs to be modified.
```
exiftool original.jpg
```

![{27F475C8-F4FA-4483-BF35-CF95D2BD7B2F}](https://github.com/user-attachments/assets/7a3014b1-6d56-46e1-9a39-b1ffa4126e96)

Some need to be modified. So go and change all of that and submit it through netcat. You will have 6 tags to modify using exiftool:
- ModifyDate
- DateTimeOriginal
- CreateDate
- SubSecCreateDate
- SubSecDateTimeOriginal
- SubSecModifyDate

The way I do it is to figure out which tag needs to modify the timestamp in all the visible tags that you can see using exiftool, and submit it, and find out the next tag that needs to modify. The full command is:
```
exiftool -overwrite_original \
  "-CreateDate=1970:01:01 00:00:00.001" \
  "-ModifyDate=1970:01:01 00:00:00.001" \
  "-DateTimeOriginal=1970:01:01 00:00:00.001" \
  "-SubSecCreateDate=1970:01:01 00:00:00.001" \
  "-SubSecDateTimeOriginal=1970:01:01 00:00:00.001"\
  "-SubSecModifyDate=1970:01:01 00:00:00.001 " original.jpg
```
All can be modified except for the last tag.

![{4667D767-A245-43C5-A532-B0701ECA62C3}](https://github.com/user-attachments/assets/203f1287-1300-4433-a747-08b6722a42de)

I found the Samsung tag using exiftool. Afterward, I tried to modify it using exiftool, but no luck.
```
exiftool -Samsung:TimeStamp original.jpg
```
![{C9AC23B5-E33F-43FB-9676-63CFCA4FD892}](https://github.com/user-attachments/assets/0702b9d6-0e72-4a40-afd1-7ea8f5e8a297)

I did a bit of Google search and found out that it was a maker note tag - meaning it's a proprietary metadata field written by Samsung cameras and can't be modified. But after a few research. How about modifying it in the raw hex!!

So I used this command to find and show the timestamp:
```
exiftool -v2 original.jpg | grep -A3 TimeStamp
```
![{F4457D9B-2C1A-4CCB-A0E0-48D48218AFB9}](https://github.com/user-attachments/assets/201bda69-2b99-4d19-89e8-5f1f34f98aad)


The timestamp is written as a 13-byte binary value - a Unix timestamp in milliseconds, so use CyberChef to decode it.

![{B173E52F-91F8-4CA6-88C3-758AB23172FF}](https://github.com/user-attachments/assets/b4fa662a-f5e6-455b-a4eb-1f02377da695)

The date seems a little off compared to the previous Samsung Timestamp, but that matches. So now convert that UNIX timestamp to a hex value and find it inside the image. 

![{27555D5D-3AAA-44A9-9AC7-01A4B9FDD18C}](https://github.com/user-attachments/assets/fdba4f67-65bc-4f99-a0a8-188c0a279c39)

Found it!!

Next, we need to convert 1970:01:01 00:00:00.001+00:00 to UNIX timestamp in seconds and to hex to overwrite the timestamp value.

1970:01:01 00:00:00.001+00:00 => 1 in UNIX timestamp in seconds => 31 in hex.

So change the timestamp to "__31 00 00 00 00 00 00 00 00 00 00 00 00__".

![{2B34BAD6-3E30-43C1-BEF7-394B2DD66D55}](https://github.com/user-attachments/assets/e4a480ed-0e1e-4e88-8e3c-3bcbce9010d6)

Save it and do a confirmation.

![{57C9651B-B067-4255-B59A-4AB5AC6CA4B6}](https://github.com/user-attachments/assets/2492fbc4-aa44-4e91-b7c0-251d92b1fced)

The timestamp has changed to 1. Submit and get the flag.

### END!!
