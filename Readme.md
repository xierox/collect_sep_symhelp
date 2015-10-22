## About
These Symantec Endpoint Protection (SEP) Host Integrity (HI) policies can be used to run the SymHelp log collection tool on Windows computers without requiring an admin to interrupt the user to run the tool.

The SymHelp log files (.sdbz files) are written to a static path, which makes remote log collection simpler. The path is: **C:\WINDOWS\TEMP\symhelphi**

##  Requirements
- Symantec Endpoint Protection version 12.1.5 and higher
- Windows Server or Desktop versions (both 32- and 64-bit)

##  Setup instructions
- Download the DAT files by clicking the **Download ZIP** button
- Import both policies into your Symantec Endpoint Protection Manager (SEPM)
    - Login to the SEPM
    - Click **Policies**
    - Click **Host Integrity**
    - Click **Import a Host Integrity Policy**
- Create the group **My Company\SymHelp Collect**, disable policy inheritance, and apply the policy named **SymHelp Collect** to it
- Create the group **My Company\SymHelp Reset**, disable policy inheritance, and apply the policy named **SymHelp Reset** to it

##  Remotely creating a SymHelp log
- To create a SymHelp log on a SEP client, move the client into the group **My Company\SymHelp Collect**
- After the next heartbeat, the client will enable logging, wait 60 minutes, and then run SymHelp. You can generally expect the logs to be ready to collection at **C:\WINDOWS\TEMP\symhelphi** after about 90 minutes.

**Note:** These policies have logic in them to prevent a SymHelp log from being created on every HI check. If you need to gather a second or third SymHelp log, you must reset the SEP client first. See steps below.

##  Resetting the SEP client
Before these HI policies can be used to gather more than one SymHelp log, you must first reset the client using the steps below.

- Move the client to the group **My Company\SymHelp Reset**
- Shortly after the client heartbeats into the SEPM and picks up the new policy, it will reset its state. You can then move the client back into the other group to create another SymHelp log.

##  Tips
- Changing the communication mode of the **SymHelp Collect** and **SymHelp Reset** groups to **Push mode** will speed this process up significantly.
- The HI policy **SymHelp Reset** resets the client by setting *SymHelpExecuted* to *0*. This can be done by hand, if you so desire. It is located at: **HKEY_LOCAL_MACHINE\SOFTWARE\Symantec**
