# TwinCAT 3.1.4026 Notes
This is a personal repo for personal use relating to TwinCAT 3.1.4026, I'm adding it here so other people can read it as well. This repo is not officially Beckhoff. If you do need further help with TwinCAT, please contact your local Beckhoff Subsidiary/Representatives.  

### - Content:
- Migration via Command Line
- Connecting to 4024 Target
- TC2 and TC3.1.4026 
- Uninstalling 4026
- Downgrade to 4024

## Migration via Command Line
>[!Important]
> - Please **DO NOT** use a VPN
> - Please read through all steps prior to starting
- The steps below are completely via command line.
- Attempt installing the GUI the default way prior to this method.
- The steps below are in case IT security policy are preventing Admin rights.
	- the GUI open command line multiple times in the background.

1. Start the Command Prompt as Administrator
2. `cd` to directory where the `Package Manager GUI.exe` is located
3. Execute the `Package Manager GUI.exe NO4024CHECK="true"` 
4. Check if the package manager can see any feeds `tcpkg list`
	1. if not, run the following, fill in square brackets with necessary data
```shell
tcpkg source add -n="Beckhoff Stable Feed" -s=https://public.tcpkg.beckhoff-cloud.com/api/v1/feeds/stable --priority=[UniqueValue, highest is 1] -u=[myBeckhoff Email] --password=[myBeckhoff Password]
```
5. Install TcMigrateCli package
```shell
tcpkg install TwinCAT.XAE.MigrateCli
```
6. You can skip this step if you want, Run the migration in test mode
```shell
TcMigrateCmd upgrade
```
6. Then run the migration without testing
```shell
TcMigrateCmd upgrade --whatIf False
```
8. This will start the migration
	- Best time to have a coffee because this will take a while
9. Halfway through, your computer will request to restart
	1. When it restarts **DO NOT DO ANYTHING**
	2. A new command prompt should open/be requested to open and will continue the migration
	3. This should attempt to reinstall the equivalent 4026 version of 4024 packages you had installed
>[!Important]
> - if this is the first time your laptop/desktop/machine is using TwinCAT
> - Open TwinCAT via the Windows 11 taskbar system tray the first time
### - Additional Command Line Commands
 https://infosys.beckhoff.com/content/1033/tc3_installation/15698626059.html?id=5147078465983576506
 
## Control/Connect to a 4024 XAR via 4026 XAE
- due to the significant difference between v4026 and v4024
- A remote manager will need to be used
- As of current (22/8/2025), the remote manager for v4024 and older has been moved to their own feed called "Outdated"

1. Open Package Manager
2. Open Settings
3. Go to `Feeds`
4. Add `Outdated` from the dropdown list and that should autofill the rest apart from login credentials
![alt text](images/OutdatedFeed.png)
5. Now `Remote Manager` workload should have mutliple options for 4024,4022,4020,4018
![alt text](images/RMoptions.png)
### - Via command line
- To add Outdated feed to source lise
```shell
tcpkg source add -n="Beckhoff Outdated Feed" -s=https://public.tcpkg.beckhoff-cloud.com/api/v1/feeds/outdated --priority=1 -u=[UserName] --password=[myBeckhoff Password]
```
## TwinCAT 2 and TwinCAT 3.1.4026 on same machine
- When moving to TC3.1 v4026
- If you had TwinCAT 2 installed, it would have been removed as well
- TwinCAT 2 can be reinstalled from the Package manager
- You'll need to make sure the `Outdated` feed is activated
- It won't be a `Workload` so you will need to swap to `Packages` view to find it
	- This is the top left button
- Seach `TwinCAT.XAE.TC2Engineering` and it should show up
- Download those packages and TwinCAT 2 options should appear in the System Icon as options  
![alt text](images/TC2options.png)
## Uninstalling TwinCAT 3.1.4026
- Rather than going via "Add or Remove Programs" in Windows
1. Start the command prompt as Administrator
2. Check what is installed: 
```shell
tcpkg list -i
```
3. Then uninstall: 
```shell
tcpkg uninstall all
```
4. Remove `Beckhoff` directory from `Program Data`,`Program Files`, `Program Files(x86)`

## Downgrade from TwinCAT 3.1.4026 to TwinCAT 3.1.4024
- If you try to run a 4024 .exe with v4026 installed, you'll get an error stating "New version is installed"
- **DO NOT** remove 4026 via "Add or Remove Programs"
- Follow the steps above with `Commnad Prompt`
- Then download the 4024 .exe
- If you have a partial install of v4026, you'll need to complete it prior to uninstalling to confirm everything has been removed
