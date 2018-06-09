# escape=`

FROM microsoft/windowsservercore as builder

ENV ARIA2_VERSION 1.34.0
ENV ARIA2_DOWNLOAD_URL "https://github.com/aria2/aria2/releases/download/release-$ARIA2_VERSION/aria2-$ARIA2_VERSION-win-64bit-build1.zip"

WORKDIR /aria2
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12; `
        Invoke-WebRequest $env:ARIA2_DOWNLOAD_URL -OutFile aria2.zip
RUN Expand-Archive aria2.zip -DestinationPath .\ ; `
        Move-Item .\aria2-$env:ARIA2_VERSION-win-64bit-build1\aria2c.exe .\aria2c.exe

FROM microsoft/nanoserver

COPY --from=builder ["/aria2/aria2c.exe", "/aria2c.exe"]
WORKDIR /

ENTRYPOINT ["aria2c", "--conf-path", "/aria2/aria2.config"]

