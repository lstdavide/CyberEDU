# zebra-lib
###### Category: Misc
###### Difficulty: Easy
###### Points: 50
###### Description: All these years of technological developments and I still haven’t seen a color photo of a zebra. Change my mind.
---
##### Walktrough: 
Starting the challenge instance, we are given an address. There is no imediate response on web so we can use Netcat on the address.

<img width="823" height="146" alt="Screenshot from 2026-09-17 20-28-48" src="https://github.com/user-attachments/assets/72272020-499f-4e5f-8d3f-b0071f382539" />

The output of the instance changes some bits every iteration so we use CyberChef to try and decode.  CyberChef Magic uses From Base64 and Zlib Inflate so we identify that we need to use zebra-lib. (title also confirms this)

<img width="688" height="564" alt="Screenshot from 2026-09-17 20-42-52" src="https://github.com/user-attachments/assets/f5c0f0e5-c13a-494d-ac5e-ec1ef1055738" />

The outputs are valid but we need multiple iterations for a confirmed response, therefore we automate with a custom script in Python.

<img width="796" height="564" alt="Screenshot from 2026-09-17 21-04-31" src="https://github.com/user-attachments/assets/060f5990-9ace-4ad3-b7b6-d1b3971d5bd1" />

To run the script, we create a virtual environment, install "pwntools" and run the script.

<img width="866" height="542" alt="Screenshot from 2026-09-17 21-10-11" src="https://github.com/user-attachments/assets/4e45a4e0-9dc8-4a33-8fa7-114f1f877313" />

<img width="850" height="921" alt="Screenshot from 2026-09-17 21-10-45" src="https://github.com/user-attachments/assets/590a881f-a1c4-4550-a648-14353d9cd444" />
