# escape=`

FROM mcr.microsoft.com/windows/servercore:1909
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# From https://github.com/dotnet/dotnet-docker/blob/74c92451ecbd2876280ad51736a6eea4e98a1fb2/3.1/runtime/nanoserver-1909/amd64/Dockerfile
# Retrieve .NET Core Runtime
RUN $dotnet_version = '3.1.2'; `
    Invoke-WebRequest -OutFile dotnet.zip https://dotnetcli.azureedge.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-win-x64.zip; `
    $dotnet_sha512 = '2986d9f04640115cfa1ec478ac20d8e5c0f3b57f1d4e379300dfbafe8d20ec775ea1987045d611a1b2d786de7e1981c57ef52824a9d8dda64e1570379187b23f'; `
    if ((Get-FileHash dotnet.zip -Algorithm sha512).Hash -ne $dotnet_sha512) { `
        Write-Host 'CHECKSUM VERIFICATION FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive dotnet.zip -DestinationPath dotnet; `
    Remove-Item -Force dotnet.zip

# set system PATH to persist across sessions.
RUN $Env:PATH = 'C:\Program Files\dotnet;' + $Env:PATH; `
    [Environment]::SetEnvironmentVariable('PATH', $Env:PATH, [EnvironmentVariableTarget]::Machine)

# Enable detection of running in a container
ENV DOTNET_RUNNING_IN_CONTAINER=true