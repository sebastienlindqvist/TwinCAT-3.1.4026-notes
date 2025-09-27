# TwinCAT 3.1.4026 | OPC UA
- This repo is not officially Beckhoff. If you do need further help with TwinCAT, please contact your local Beckhoff Subsidiary/Representatives.
- OPC Unified Architecture (UA) is a platform-independent, service-oriented protocol design that integrates all the functionality of the individual OPC Classic specifications into one extensible specification framework.

- As of current(25/8/2025) this is the following allowed OS per software:  

| OS             | Client | Configurator | Gateway | Server | Pub/Sub | Node Set Editor |
| -------------- | ------ | ------------ | ------- | ------ | ------- | --------------- |
| Windows CE6/7  | ✔️     | ❌            | ❌       | ✔️     | ❌       | ❌               |
| WES7           | ❌      | ❌            | ❌       | ❌      | ✔️      | ❌               |
| Windows 7      | ❌      | ❌            | ✔️      | ❌      | ✔️      | ❌               |
| Windows 10     | ✔️     | ✔️           | ✔️      | ✔️     | ✔️      | ✔️ 4026 only    |
| Windows 11     | ✔️      | ✔️            | ✔️       | ✔️      | ✔️       | ✔️ 4026 only    |
| Windows Server | ✔️     | ✔️           | ✔️      | ✔️     | ❌       | ❌               |
| Tc/BSD         | ❌      | ❌            | ❌       | ✔️     | ❌       | ❌               |
| Tc/Linux       | ❌      | ❌            | ❌       | ✔️     | ❌       | ❌               |

## How to setup an OPC UA server
- Open Package Manager on the machine you want the server on
- Search for TF6100 Server and Install
- On your Dev laptop
- Create a Connectivity Project
	- Add an OPC UA Server Project

- If you don't see the OPC UA configurator task bar, right click and add to the task bar
- add server to server list
	- delete any pre-existing ones
- Choose the Server from the drop and click "Connect"

- click the cheque box and then Trust

- Give your username and password. Be careful, if the username you put in is already on the OS, it will use that password

- disconnect and reconnect

- add username and password you just created

- for you first time, click yes. later on it depends if connectivity project is more developed than the server itself

- Click Yes

- The solution explorer should appear to change and add additional DataAccess, SecurityAcess stuff
## Common Issues
- If you get an error saying `BadCommunicationError`?
    - Check if TwinCAT XAR is `Activated `and opened port 851 by `Logging in`
- Want to host the server on the IPC but programming it via a laptop?
Make sure the server you're trying to connect to is   
    - not set to: `Localhost:4840`  
    - But rather: `IPaddressOfIPC:4840`

## OPC UA - Debugging
- this steps below are for Support to help with debugging an issue

### step 1: Installers & executables...
- Have they installed the software.....
	- this happens more than it should

- In order for a CX/IPC to have a OPC Server, the TF6100 server .exe must be ran on the CX/IPC
	- for 4024, this also has a checkbox to allow None/None endpoint
		- i.e. no encryption or security
	- once installed, you should see a purple icon
	- As well as TF6100 should be in the Functions directory of TwinCAT

- In order to configure the Server, the TF6100 configurator .exe must be installed on the device that is being used to configure the CX/IPC
	- it can be installed anywhere but for ease, do it on the same machine where TwinCAT XAE is being used.

- In order for TwinCAT to be a client to a OPC UA server. The TF6100 client must be installed installed on ...................................

### step 2: Networking
- Can they ping the device wanting to host the sever?
- is port 4840 open on both sides?
	- open powershell and try:
```
tnc <ip address> -port 4840
```
### step 3: TwinCAT Project and Connecitivity Project/Configurator
- Is the TMC checkbox ticked
	- If TC2, is the TPY checkbox ticked
- Attribute is added 
```
{attribute 'OPC.UA.DA' := '1'}
```
### step 4: if we're the client
- we have 2 methods of being a client
	- Virtual IO
	- openPLC code
- confirm what kind of certificate is used.
	- This is the main issue usually
	- try with Anonymous and None/None endpoints

- TF6100 client needs to be installed on the dev laptop and CX/IPC running the program
- If you don't, you'll get an error saying the "... TcIoOpcUa server. Win32 Error:1060 (the specified service does not exist as an installed service)."

- In order to write to the server from OPC virtual client
	- client output called "Execute" needs to be toggled to 1
	- that will allow you to write to the server.