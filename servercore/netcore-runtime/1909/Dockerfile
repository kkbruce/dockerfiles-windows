# escape=`

FROM mcr.microsoft.com/windows/servercore:1909
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Source From https://github.com/dotnet/dotnet-docker/blob/5e9b849a900c69edfe78f6e0f3519009de4ab471/3.1/runtime/nanoserver-1909/amd64/Dockerfile
# Retrieve .NET Core Runtime
RUN $dotnet_version = '3.1.3'; `
    Invoke-WebRequest -OutFile dotnet.zip https://dotnetcli.azureedge.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-win-x64.zip; `
    $dotnet_sha512 = '62a18838664afd6f08cdb9a90a96a67626743aab1f0de0065eadfd7d1df31681c90f96744ccb5b7e40834c9e3c4952125c8c83625867c500692050cdd113ff50'; `
    if ((Get-FileHash dotnet.zip -Algorithm sha512).Hash -ne $dotnet_sha512) { `
        Write-Host 'CHECKSUM VERIFICATION FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive dotnet.zip -DestinationPath dotnet; `
    Remove-Item -Force dotnet.zip

# set system PATH to persist across sessions.
RUN $Env:PATH = 'C:\dotnet;' + $Env:PATH; `
    [Environment]::SetEnvironmentVariable('PATH', $Env:PATH, [EnvironmentVariableTarget]::Machine)

# Enable detection of running in a container
ENV DOTNET_RUNNING_IN_CONTAINER=true