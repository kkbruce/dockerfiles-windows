# escape=`

FROM mcr.microsoft.com/windows/servercore:2004
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Source From https://github.com/dotnet/dotnet-docker/blob/11a446f2826c2b8c51baa774584ff3f28ba0e88e/src/runtime/3.1/nanoserver-2004/amd64/Dockerfile
# Retrieve .NET Core Runtime
RUN $dotnet_version = '3.1.7'; `
    Invoke-WebRequest -OutFile dotnet.zip https://dotnetcli.azureedge.net/dotnet/Runtime/$dotnet_version/dotnet-runtime-$dotnet_version-win-x64.zip; `
    $dotnet_sha512 = '51ef166e9c935ee236b16aa2f87e985664740de7f4a53c1c1becc04a42d7c7cd52afa652d9b2e477a014f959b349826d6f8c2f703ab11b312fcd202b0b22d1ca'; `
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