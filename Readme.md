# About
These Symantec Endpoint Protection (SEP) Host Integrity (HI) policies can be used to run the SymHelp log collection tool on client computers without requiring an admin to interrupt the user to run the tool.

The SymHelp log files (.sdbz files) are written to a static path, which makes remote log collection simpler. The path is: **C:\WINDOWS\TEMP\symhelphi**

# Requirements
- Symantec Endpoint Protection version 12.1.5 and higher

# Setup instructions
- Download both DAT files
- Import both policies into your Symantec Endpoint Protection Manager (SEPM)
    - Login to SEPM
    - Click Policies
    - Click Host Integrity
    - Click Import a Host Integrity Policy
- Create the group: **My Company\Collect SymHelp** and apply the policy named **Collect SymHelp** to it
- Create the group: **My Company\Reset SymHelp** and apply the policy named **Reset SymHelp** to it

# Remotely creating a SymHelp log
- To create a SymHelp log on a SEP client, move the client into the group: **My Company\Collect SymHelp**
- Shortly after the client heartbeats into the SEPM and picks up the new policy, it will create *one* SymHelp log. The SymHelp log file (.sdbz) is written to: **C:\WINDOWS\TEMP\symhelphi**

**Note:** These policies have logic in them to prevent a SymHelp log from being gathered on every HI check. If you need to gather a second or third SymHelp log, you must reset the SEP client first. See steps below.

# Resetting the SEP client

Before these HI policies can be used to gather more than one SymHelp log, you must first reset the client using the steps below.

- Move the client to the group: **My Company\Reset SymHelp**
- Shortly after the client heartbeats into the SEPM and picks up the new policy, it will reset its state. You can then move the client back into the group to gather another SymHelp log.

# Tips
- Reducing the heartbeat interval of the **Collect SymHelp** and **Reset SymHelp** groups will speed this process up significantly.
- The HI policy **Reset SymHelp** just resets the client by setting *SymHelpExecuted* to *0*. This can be done by hand, if you so desire. It is located at: **HKEY_LOCAL_MACHINE\SOFTWARE\Symantec**
