Simple Distributed Deployment Tool
-----------------------------------------------------------------------
* https://github.com/vanthoainguyen/DistributedDeploymentTool

##1. INTRODUCTION

This is a small simple console x64 application tool which can

* Act as a client to execute a command on remote server
* Act as a server to listen on a specific port for the remote commands from client
* Act as a console app to send "exit" signal to services running on the same box
* Serve as a .NET dll which exposed following interface:

```c#
public interface ISignalListener
{
    void Exit();
}
```

Your app should implement this interface and listen on the Exit signal from this utility to facilitate the application gently exit. If you need x86 version, please compile from source ;)


##2. USAGES
* Execute a command on remote server:
The server should listen on a port(default 5555) and specify a security token.

Server: execute this line

>  _DD -listen 5555 -token YourS3cur!tyTok3n_


Client: simply send a command with the same token

>  _DD -execute C:\SomeFolder\DeployAwesomeService.bat -token YourS3cur!tyTok3n_

Install as a service: 

>  _C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe /listen=5555 /token=YourS3cur!tyTok3n /i DD.exe_

Uninstall window service:

>  _C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe /listen=5555 /token=YourS3cur!tyTok3n /u DD.exe_


* Send "Exit" signal to an app in the same box

Firstly, your app has to implement above interface. Assume that your service is "LongRunningService" and in the Program.cs, run following code
```c#
var svc = new LongRunningService();
Server.Start<ISignalListener>(svc, "Any_thing_that_make_your_service_unique");
```

Then whenever you want the "LongRunningService" to exit gently such as wait for all processing threads to finish which you should implement in the Exit method,
run following command:


>  _DD -s Any_thing_that_make_your_service_unique -w 10 --kill LongRunningService_

The --kill is optional, it will try to kill the process by provided process name which is "LongRunningService" so take attention if you have multiple processes of the same service.

##3. LICENCE
http://sam.zoy.org/wtfpl/COPYING 
![Troll](http://i40.tinypic.com/2m4vl2x.jpg) 