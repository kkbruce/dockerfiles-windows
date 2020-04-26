# escape=`

FROM mcr.microsoft.com/windows/servercore:1909
LABEL maintainer="kkbruce" version="1.18.0"

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# .zip file URL from http://nginx.org/en/download.html
RUN Invoke-WebRequest -Uri http://nginx.org/download/nginx-1.18.0.zip -OutFile nginx.zip ; `
	Expand-Archive nginx.zip -DestinationPath $Env:ProgramFiles ; `
	Remove-Item -Force nginx.zip ; `
	Move-Item $Env:ProgramFiles\nginx-* $Env:ProgramFiles\nginx

RUN $Env:PATH = 'C:\Program Files\nginx;' + $Env:PATH; `
    [Environment]::SetEnvironmentVariable('PATH', $Env:PATH, [EnvironmentVariableTarget]::Machine)

HEALTHCHECK --interval=60s `
            CMD powershell -command `
            try { `
                $request=Invoke-WebRequest -Uri http://localhost/ -UseBasicParsing; `
                if ($request.StatusCode -eq 200) `
                    {return 0} `
                else `
                    {return 1}; `
            } catch {return 1}

EXPOSE 80 443
WORKDIR "C:\Program Files\nginx"
CMD Start-Process -NoNewWindow -FilePath nginx.exe ; `
	Add-Content logs\access.log 'nginx started...' ; `
    Add-Content logs\error.log ''; `
    Get-Content -Wait logs\error.log