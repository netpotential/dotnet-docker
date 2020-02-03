# escape=`
FROM mcr.microsoft.com/windows/servercore:1809

MAINTAINER dealdiane@netpotential.co.nz

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# set up environment
ENV NODE_VERSION 10.16.3
ENV NODE_DOWNLOAD_SHA 19AA47DE7C5950D7BD71A1E878013B98D93871CC311D7185F5472E6D3F633146
ENV GIT_VERSION 2.15.1
ENV GIT_PATCH_VERSION 2
ENV GIT_DOWNLOAD_SHA 2D2F3AC49CAF7296DD4299236CA9425DC11954B3486F5A3301BF7B7A63286DBC

# Install node
RUN [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; `
    Invoke-WebRequest https://nodejs.org/dist/v${env:NODE_VERSION}/node-v${env:NODE_VERSION}-win-x64.zip -OutFile node.zip -UseBasicParsing; `
    if ((Get-FileHash node.zip -Algorithm sha256).Hash -ne $env:NODE_DOWNLOAD_SHA) { `
        Write-Host 'NODEJS CHECKSUM VERIFICATION FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive node.zip -DestinationPath C:\nodejs; `
    Move-Item C:\nodejs\node-v${env:NODE_VERSION}-win-x64 ${env:ProgramFiles}\nodejs; `
    Remove-Item -Force C:\nodejs; `
    Remove-Item -Force node.zip;
    
# Install git
RUN [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 ; `
    Invoke-WebRequest https://github.com/git-for-windows/git/releases/download/v${env:GIT_VERSION}.windows.${env:GIT_PATCH_VERSION}/MinGit-${env:GIT_VERSION}.${env:GIT_PATCH_VERSION}-busybox-64-bit.zip -OutFile mingit.zip -UseBasicParsing; `
    if ((Get-FileHash mingit.zip -Algorithm sha256).Hash -ne $env:GIT_DOWNLOAD_SHA) { `
        Write-Host 'GIT CHECKSUM VERIFICATION FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive mingit.zip -DestinationPath c:\mingit ; `
    Move-Item C:\mingit ${env:ProgramFiles}\git; `
    Remove-Item mingit.zip -Force;