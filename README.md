# [Buffer-Overflow-Prep](https://tryhackme.com/room/bufferoverflowprep) 

<h1>This is writeup for Buffer-Overflow-Prep room in tryhackme.com</h1>

<h2>1. __Tasks__</h2>

  <h3>[Task 1] Deploy VM</h3>
  
  This room uses a 32-bit Windows 7 VM with Immunity Debugger and Putty preinstalled. Windows Firewall and Defender have both been disabled to make exploit writing easier.

You can log onto the machine using RDP with the following credentials: admin/password

I suggest using the xfreerdp command: 

    ✅ xfreerdp /u:admin /p:password /cert:ignore /v:MACHINE_IP

For full screen you can use /workarea : Use available work area. So you will see properly and in a comfortable way as the size is small in Immunity Debugger.

    ✅ xfreerdp /u:admin /p:password /cert:ignore /v:MACHINE_IP /workarea
