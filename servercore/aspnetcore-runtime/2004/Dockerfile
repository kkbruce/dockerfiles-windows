# escape=`

FROM mcr.microsoft.com/windows/servercore:2004
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Source from https://github.com/dotnet/dotnet-docker/blob/11a446f2826c2b8c51baa774584ff3f28ba0e88e/src/aspnet/3.1/nanoserver-2004/amd64/Dockerfile
# Install ASP.NET Core Runtime
RUN $aspnetcore_version = '3.1.7'; `
    Invoke-WebRequest -OutFile aspnetcore.zip https://dotnetcli.azureedge.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-win-x64.zip; `
    $aspnetcore_sha512 = 'f330c8b02699340503d4129626c0290097dc79d5d5cf97941ce9344f78de5e9bd3cba1a726b56753db0cac9db8b531c21335a3ea04dc740f09e2e5327e9f423e'; `
    if ((Get-FileHash aspnetcore.zip -Algorithm sha512).Hash -ne $aspnetcore_sha512) { `
        Write-Host 'CHECKSUM VERIFICATION FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive aspnetcore.zip -DestinationPath dotnet; `
    Remove-Item -Force aspnetcore.zip

RUN $Env:PATH = 'C:\dotnet;' + $Env:PATH; `
    [Environment]::SetEnvironmentVariable('PATH', $Env:PATH, [EnvironmentVariableTarget]::Machine)

# Configure web servers to bind to port 80 when present
ENV ASPNETCORE_URLS=http://+:80 `
    # Enable detection of running in a container
    DOTNET_RUNNING_IN_CONTAINERS=true