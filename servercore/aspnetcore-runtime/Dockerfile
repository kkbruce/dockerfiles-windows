# escape=`
# FROM mcr.microsoft.com/windows/servercore:1709
# FROM mcr.microsoft.com/windows/servercore:1803
# FROM mcr.microsoft.com/windows/servercore:1809
FROM mcr.microsoft.com/windows/servercore:1809
LABEL maintainer="kkbruce" platform="windows" os="servercore"
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

ENV ASPNETCORE_VERSION=2.2.5

WORKDIR C:\

RUN Invoke-WebRequest -OutFile aspnetcore.zip https://dotnetcli.blob.core.windows.net/dotnet/aspnetcore/Runtime/$Env:ASPNETCORE_VERSION/aspnetcore-runtime-$Env:ASPNETCORE_VERSION-win-x64.zip; `
    $aspnetcore_sha512 = 'aa8802c8c482d2fc708e669d9e9f0de7e74700afb580f0197545f33b50d67317fea7101a4704b2c35b1362d92be814e3fd31436cabf53f2e7755f8953278cdd3'; `
    if ((Get-FileHash aspnetcore.zip -Algorithm sha512).Hash -ne $aspnetcore_sha512) { `
        Write-Host 'CHECKSUM VERIFICATION FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive aspnetcore.zip -DestinationPath 'C:\Program Files\dotnet'; `
    Remove-Item -Force aspnetcore.zip; `
    $env:PATH = 'C:\Program Files\dotnet;' + $env:PATH; `
    [Environment]::SetEnvironmentVariable('PATH', $env:PATH, [EnvironmentVariableTarget]::Machine)

ENV ASPNETCORE_URLS=http://+:80 `
    DOTNET_RUNNING_IN_CONTAINERS=true