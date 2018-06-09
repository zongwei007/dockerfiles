# escape=`

FROM microsoft/windowsservercore as builder

ENV FRP_VERSION 0.17.0
ENV FRP_DOWNLOAD_URL "https://github.com/fatedier/frp/releases/download/v$FRP_VERSION/frp_${FRP_VERSION}_windows_amd64.zip"

WORKDIR /frp
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12; `
        Invoke-WebRequest $env:FRP_DOWNLOAD_URL -OutFile frp.zip
RUN Expand-Archive frp.zip -DestinationPath .\ ; `
        Move-Item ".\frp_${env:FRP_VERSION}_windows_amd64\frpc.exe" .\frpc.exe

FROM microsoft/nanoserver

COPY --from=builder ["/frp/frpc.exe", "/frpc.exe"]
WORKDIR /

CMD ["frpc"]
