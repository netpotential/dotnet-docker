# escape=`
FROM microsoft/windowsservercore:1803

MAINTAINER dealdiane@netpotential.co.nz

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# set up environment
ENV NODE_VERSION 8.10.0
ENV NODE_DOWNLOAD_SHA 936ADA36CB6F09A5565571E15EB8006E45C5A513529C19E21D070ACF0E50321B
ENV GIT_VERSION 2.18.0
ENV GIT_DOWNLOAD_SHA C2F59C121D0F5AAC31C959E5BA2878542B6CBCA6604778566061D45585E70895

# Install node
RUN [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; `
    Invoke-WebRequest -UseBasicParsing https://nodejs.org/dist/v${env:NODE_VERSION}/node-v${env:NODE_VERSION}-win-x64.zip -outfile node.zip; `
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
RUN [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; `
	Invoke-WebRequest -UseBasicParsing https://github.com/git-for-windows/git/releases/download/v${env:GIT_VERSION}.windows.1/MinGit-${env:GIT_VERSION}-32-bit.zip -OutFile git.zip; `
	if ((Get-FileHash git.zip -Algorithm sha256).Hash -ne $env:GIT_DOWNLOAD_SHA) { `
        Write-Host 'GIT CHECKSUM VERIFICATION FAILED!'; `
        exit 1; `
    }; `
    `
    Expand-Archive git.zip -DestinationPath "C:/git"; `
    Move-Item C:\git ${env:ProgramFiles}\git; `
    Remove-Item -Force C:\git; `
    Remove-Item -Force git.zip;