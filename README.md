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

If Windows prompts you to choose a location for your network, choose the "Home" option.

On your Desktop there should be a folder called "vulnerable-apps". Inside this folder are a number of binaries which are vulnerable to simple stack based buffer overflows (the type taught on the PWK/OSCP course):

The SLMail installer.
The brainpan binary.
The dostackbufferoverflowgood binary.
The vulnserver binary.
A custom written "oscp" binary which contains 10 buffer overflows, each with a different EIP offset and set of badchars.
I have also written a handy guide to exploiting buffer overflows with the help of mona: [https://github.com/Tib3rius/Pentest-Cheatsheets/blob/master/exploits/buffer-overflows.rst](https://github.com/Tib3rius/Pentest-Cheatsheets/blob/master/exploits/buffer-overflows.rst)

Please note that this room does not teach buffer overflows from scratch. It is intended to help OSCP students and also bring to their attention some features of mona which will save time in the OSCP exam.

Thanks go to [@Mojodojo_101](https://twitter.com/Mojodojo_101) for helping create the custom oscp.exe binary for this room!


