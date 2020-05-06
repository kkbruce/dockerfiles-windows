# escape=`

FROM mcr.microsoft.com/windows/servercore:1803
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Source from https://github.com/dotnet/dotnet-docker/blob/5e9b849a900c69edfe78f6e0f3519009de4ab471/3.1/aspnet/nanoserver-1909/amd64/Dockerfile
# Install ASP.NET Core Runtime
RUN $aspnetcore_version = '3.1.3'; `
    [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; `
    Invoke-WebRequest -OutFile aspnetcore.zip https://dotnetcli.azureedge.net/dotnet/aspnetcore/Runtime/$aspnetcore_version/aspnetcore-runtime-$aspnetcore_version-win-x64.zip; `
    $aspnetcore_sha512 = '6d8a21a7420db9091fc05613ef0a923c7b86cc995360c9f0133d632020e042db0efac0343ee6516a28feb2c1dd004b33b74bfdcc1a687efdefa9db1a486c1ca2'; `
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