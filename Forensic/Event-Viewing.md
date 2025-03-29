### TITLE
>Event-Viewing
### DESCRIPTION
> One of the employees at your company has their computer infected by malware! Turns out every time they try to switch on the computer, it shuts down right after they log in. The story given by the employee is as follows:

> 1. They installed software using an installer they downloaded online

> 2. They ran the installed software but it seemed to do nothing

> 3. Now every time they bootup and login to their computer, a black command prompt screen quickly opens and closes and their computer shuts down instantly.

> See if you can find evidence for the each of these events and retrieve the flag (split into 3 pieces) from the correct logs!
### CATEGORY
> Forensic
### HINT
>Try to filter the logs with the right event ID.

> What could the software have done when it was ran that causes the shutdowns every time the system starts up?
### DIFFICULTY
> medium
### AUTHOR
> Venax
### TOOLS
> [cyberchef](https://cyberchef.org)
### FLAG
> picoCTF{Ev3nt_vi3wv3r_1s_a_pr3tty_us3ful_t00l_81ba3fe9}
### SOLVED
So you need to find 3 flag pieces based on those 3 evidence.
Open the file using Event Viewer => Filter current log => Type in the event ID (Flag being encoded in Base64 string so find and decode it using Cyberchef)
1. Find the installation of a software (ID: 1033)

![image](https://github.com/user-attachments/assets/5df8ae57-a4cb-4f92-a49f-66e0da7d721d)

2. Find a registry value was modified (ID: 4657)

![image](https://github.com/user-attachments/assets/7fc55271-aca7-44b6-ba62-1a08312a580e)

3. Find the shutdown (ID: 1074)

![image](https://github.com/user-attachments/assets/21a51f84-62de-4bf5-8d19-4d0a8233ba80)

__P/S__: 1,2 You can find it using Ctrl+F and find the proccess name of: __Totally_Legit_Software__.
