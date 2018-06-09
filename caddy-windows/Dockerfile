FROM microsoft/windowsservercore as builder

ENV CADDY_VERSION 0.11.0
ENV CADDY_DOWNLOAD_URL "https://github.com/mholt/caddy/releases/download/v$CADDY_VERSION/caddy_v${CADDY_VERSION}_windows_amd64.zip"

WORKDIR /caddy
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12; \
        Invoke-WebRequest $env:CADDY_DOWNLOAD_URL -OutFile caddy.zip
RUN Expand-Archive caddy.zip -DestinationPath .

FROM microsoft/nanoserver

COPY --from=builder ["/caddy/caddy.exe", "/caddy.exe"]

SHELL ["powershell", "-command"]

# Make sure that Docker always uses default DNS servers which hosted by Dockerd.exe
RUN Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ServerPriorityTimeLimit -Value 0 -Type DWord; \
	Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ScreenDefaultServers -Value 0 -Type DWord; \
	Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name ScreenUnreachableServers -Value 0 -Type DWord
	
# Shorten DNS cache times
RUN Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name MaxCacheTtl -Value 30 -Type DWord; \
	Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\Dnscache\Parameters' -Name MaxNegativeCacheTtl -Value 30 -Type DWord

WORKDIR /

ENTRYPOINT ["caddy", "-conf", "/caddy/Caddyfile"]
