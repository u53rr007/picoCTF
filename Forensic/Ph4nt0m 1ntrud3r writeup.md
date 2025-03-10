### TITLE
>Ph4nt0m 1ntrud3r
### DESCRIPTION
> A digital ghost has breached my defenses, and my sensitive data has been stolen! 😱💻 Your mission is to uncover how this phantom intruder infiltrated my system and retrieve the hidden flag.
To solve this challenge, you'll need to analyze the provided PCAP file and track down the attack method. The attacker has cleverly concealed his moves in well timely manner. Dive into the network traffic, apply the right filters and show off your forensic prowess and unmask the digital intruder!
Find the PCAP file here Network Traffic PCAP file and try to get the flag.
### CATEGORY
> Forensic
### HINT
>Filter your packets to narrow down your search.

>Attacks were done in timely manner.

>Time is essential.
### DIFFICULTY
>easy
### Tools
>wireshark, tshark, [cyberchef](https://cyberchef.org/)
### FLAG
>
### SOLVED
Open the pcap file (myNetworkTraffic.pcap) and you will see there is just a bunch of TCP retransmissions nothing else. Look closely at the frame length, and you will notice that some packets have a length that is different than 48. Filter that out:
```
frame.len != 48
```
![image](https://github.com/user-attachments/assets/dec79aa2-8450-419b-bd50-99fe53e3fc40)

Because the hint mentions something related to time so sort the time too. Look even more closely at the TCP payload and there are some __base64__ encodes inside that.

![image](https://github.com/user-attachments/assets/c11b725f-bae8-4e33-b681-b20f3656be94)

Decode it!!

![image](https://github.com/user-attachments/assets/45bfd7db-3ad4-4662-8078-9c76cd6e4649)

Huh? Interesting!! Continue to use Wireshark to manually extract those values or use tshark to do so:
```
tshark -r myNetworkTraffic.pcap -Y "frame.len != 48 and tcp.payload" -T fields -e tcp.payload
```
You will get all the flag pieces. Now try to put those in the correct order and get the flag.

### END!!
