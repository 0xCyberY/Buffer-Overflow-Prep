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


   <h3>[Task 2] oscp.exe - OVERFLOW1</h3>
   
Right-click the Immunity Debugger icon on the Desktop and choose "Run as administrator".

When Immunity loads, click the open file icon, or choose File -> Open. Navigate to the vulnerable-apps folder on the admin user's desktop, and then the "oscp" folder. Select the "oscp" (oscp.exe) binary and click "Open".

The binary will open in a "paused" state, so click the red play icon or choose Debug -> Run. In a terminal window, the oscp.exe binary should be running, and tells us that it is listening on port 1337.

On your Kali box, connect to port 1337 on MACHINE_IP using netcat:

    ✅ nc MACHINE_IP 1337

Type "HELP" and press Enter. Note that there are 10 different OVERFLOW commands numbered 1 - 10. Type "OVERFLOW1 test" and press enter. The response should be "OVERFLOW1 COMPLETE". Terminate the connection.

Mona Configuration

The mona script has been preinstalled, however to make it easier to work with, you should configure a working folder using the following command, which you can run in the command input box at the bottom of the Immunity Debugger window:

    ✅ !mona config -set workingfolder c:\mona\%p


   <h4>Fuzzing</h4>
   
Create a file on your Kali box called fuzzer.py with the following contents:[Link]()

Run the fuzzer.py script using python: 

    ✅ python fuzzer.py

The fuzzer will send increasingly long strings comprised of As (up to 3000). If the fuzzer crashes the server with one of the strings, you should see an error like: "Could not connect to MACHINE_IP:1337". Make a note of the largest number of bytes that were sent.

   <h4>Crash Replication & Controlling EIP</h4>
   
Create another file on your Kali box called exploit.py with the following contents:[Link]()

Run the following command to generate a cyclic pattern of a length 400 bytes longer that the string that crashed the server (change the -l value to this):

    ✅ /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 600

Copy the output and place it into the payload variable of the exploit.py script.

On Windows, in Immunity Debugger, re-open the oscp.exe again using the same method as before, and click the red play icon to get it running. You will have to do this prior to each time we run the exploit.py (which we will run multiple times with incremental modifications).

On Kali, run the modified exploit.py script:

    ✅ python exploit.py

The script should crash the oscp.exe server again. This time, in Immunity Debugger, in the command input box at the bottom of the screen, run the following mona command, changing the distance to the same length as the pattern you created:

    ✅ !mona findmsp -distance 600

Mona should display a log window with the output of the command. If not, click the "Window" menu and then "Log data" to view it (choose "CPU" to switch back to the standard view).

In this output you should see a line which states:

    ✅ EIP contains normal pattern : ... (offset XXXX)




