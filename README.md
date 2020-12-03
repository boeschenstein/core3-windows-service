# .NET Core 3 Windows Service (Worker)

Create a new project in \WindowsServiceExample\WindowsServiceExample.proj:

`dotnet new worker --name WindowsServiceExample`

Source: https://dotnetcoretutorials.com/2019/12/07/creating-windows-services-in-net-core-part-3-the-net-core-worker-way/

Add Windows Service:

`Install-Package Microsoft.Extensions.Hosting.WindowsServices`

And add `.UseWindowsService()` to the end of HostBuilder:

```cs
        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureServices((hostContext, services) =>
                {
                    services.AddHostedService<Worker>();
                }).UseWindowsService();
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
