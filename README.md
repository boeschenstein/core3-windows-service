# .NET Core 3/5 Windows Service (Worker)

Create a new project in \WindowsServiceExample\WindowsServiceExample.proj:

`dotnet new worker --name WindowsServiceExample`

Source: https://dotnetcoretutorials.com/2019/12/07/creating-windows-services-in-net-core-part-3-the-net-core-worker-way/

Add Windows Service to listen to start/stop functionality:

`Install-Package Microsoft.Extensions.Hosting.WindowsServices`

and for logging to Windows EventLog:

`Install-Package Microsoft.Extensions.Logging.EventLog`

Add `.UseWindowsService()` to the end of HostBuilder and `.ConfigureLogging(loggerFactory => loggerFactory.AddEventLog())` for Logging to Windows:

```cs
using Microsoft.Extensions.Logging;

public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureLogging(loggerFactory => loggerFactory.AddEventLog()) // needs `using Microsoft.Extensions.Logging;`
        .ConfigureServices((hostContext, services) =>
        {
            services.AddHostedService<Worker>();
        })
        .UseWindowsService();
```

Build the service:

`dotnet publish -r win-x64 -c Release`

Installing, Starting, Stopping and Deleting is BAU (in cmd (not powershell...) running as administrator):

```
sc create TestService BinPath= "C:\full\path\to\publish\dir\WindowsServiceExample.exe"
sc start TestService
sc stop TestService
sc delete TestService
```

Delayed start:

```
sc config TestService start= delayed-auto
sc start TestService start= delayed-auto
```

> TODO: _logger.LogInformation is not visible in EventLog (_logger.LogWarning is shown)

## Information

- to debug, set both projects as startup
- Check *Windows EventLog* for Logging 
- Check *Windows Services* if service is running.

## InstallUtil not suited for .net core

Use this for .net classic:

`c:\Windows\Microsoft.NET\Framework\v4.0.30319\InstallUtil.exe "G:\INETPUB\WEBAPPS\MyApp\TestServer.exe"`

## Access issue

netsh http add urlacl url=http://+:4718/TestService.MyNamespace user=Everyone
