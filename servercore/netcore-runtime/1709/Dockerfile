# escape=`

FROM mcr.microsoft.com/windows/servercore:1709
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Retrieve .NET Core Runtime Version
ENV DOTNET_VERSION 2.2.5

# Install .NET Core Runtime
RUN Invoke-WebRequest -OutFile dotnet.zip https://dotnetcli.blob.core.windows.net/dotnet/Runtime/$Env:DOTNET_VERSION/dotnet-runtime-$Env:DOTNET_VERSION-win-x64.zip; `
    $dotnet_sha512 = 'e3cce1a8fa304aff40fa1cf6b278ceda60d341dfa25c31449decd8900b6e49fc1dac34888853eba6e9ad610580f1689b90b5e9110826eb5ce80cf781696da98c'; `
    if ((Get-FileHash dotnet.zip -Algorithm sha512).Hash -ne $dotnet_sha512) { `
        Write-Host 'CHECKSUM VERIFICATION FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive dotnet.zip -DestinationPath $Env:ProgramFiles\dotnet; `
    Remove-Item -Force dotnet.zip

# set system PATH to persist across sessions.
RUN $Env:PATH = 'C:\Program Files\dotnet;' + $Env:PATH; `
    [Environment]::SetEnvironmentVariable('PATH', $Env:PATH, [EnvironmentVariableTarget]::Machine)

# Enable detection of running in a container
ENV DOTNET_RUNNING_IN_CONTAINER=true