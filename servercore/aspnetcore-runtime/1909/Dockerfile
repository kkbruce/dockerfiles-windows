# escape=`

FROM mcr.microsoft.com/windows/servercore:1909
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Modify from https://github.com/dotnet/dotnet-docker/blob/74c92451ecbd2876280ad51736a6eea4e98a1fb2/3.1/aspnet/nanoserver-1909/amd64/Dockerfile
# Install ASP.NET Core Runtime
RUN $aspnetcore_version = '3.1.2'; `
    Invoke-WebRequest -OutFile aspnetcore.zip https://dotnetcli.azureedge.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-win-x64.zip; `
    $aspnetcore_sha512 = '2052f158ed37071ff5845ef7ea3f63853c513debd038563a78094425ee8126fa54080a3f042395d6aa862ae09b6139795c65bba3156c41c911294103cb05e9d4'; `
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