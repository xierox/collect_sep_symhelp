# About
These Symantec Endpoint Protection (SEP) Host Integrity (HI) policies can be used to enable SEP logging and run the SymHelp log collection tool without interrupting the user of the computer.

This tool will:
- Enable SEP logging and restart SEP services to enable logging
- Wait 1 hour (This is configurable.)
- Download and run the SymHelp utility to collect SEP logs
- Disable SEP logging and restart SEP services to disable logging

The SymHelp log file (extension .sdbz) is written to a static path. This makes remote log collection simple. The path is: **C:\WINDOWS\TEMP\symhelphi**

#  Requirements
- Symantec Endpoint Protection version 12.1.5 and higher
- SEP's Tamper Protection feature must be disabled (see: [TECH192023](http://www.symantec.com/docs/TECH192023))
- SEP must be installed to one of the following default paths otherwise the script will not properly restart SEP's services to enable logging. This can be corrected by adjusting the path to Smc.exe in the part of the script which restarts SEP services.
    + 32-bit OS: **C:\Program Files\Symantec\Symantec Endpoint Protection**
    + 64-bit OS: **C:\Program Files (x86)\Symantec\Symantec Endpoint Protection**
- Microsoft Windows (32-bit or 64-bit)

#  How to use this tool

### Step 1: Create special groups and assign the policies
- Download the DAT files by clicking the **Download ZIP** button
- Import both policies into your Symantec Endpoint Protection Manager (SEPM)
    - Login to the SEPM
    - Click **Policies**
    - Click **Host Integrity**
    - Click **Import a Host Integrity Policy**
- Create the group **My Company\SymHelp Collect**
    - Disable policy inheritance for the group
    - Apply the policy named **SymHelp Collect** to the group
    - Disable **Tamper Protection** for the group (see: [TECH192023](http://www.symantec.com/docs/TECH192023))
- Create the group **My Company\SymHelp Reset**
    - Disable policy inheritance for the group
    - Apply the policy named **SymHelp Reset** to the group
    - Disable **Tamper Protection** for the group (see: [TECH192023](http://www.symantec.com/docs/TECH192023))

### Step 2: Move client(s) into the special group
- To create a SymHelp log on a SEP client, move the client into the group **My Company\SymHelp Collect**
- After the next heartbeat, the client will enable logging, wait 60 minutes, and then run SymHelp. You can generally expect the logs to be ready to collection at **C:\WINDOWS\TEMP\symhelphi** after about 90 minutes.

###  Step 3 (optional): Reset the SEP client
If you need to gather a second or third SymHelp log, you must reset the SEP client first. See steps below.

- Move the client to the group **My Company\SymHelp Reset**
- Shortly after the client heartbeats into the SEPM and picks up the new policy, it will reset its state. You can then move the client back into the other group to create another SymHelp log.

#  Tips
- Changing the communication mode of the **SymHelp Collect** and **SymHelp Reset** groups to **Push mode** will speed this process up significantly.
