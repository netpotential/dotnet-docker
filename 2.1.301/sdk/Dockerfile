# escape=`
FROM microsoft/dotnet:2.1.301-sdk

MAINTAINER dealdiane@netpotential.co.nz

FROM microsoft/windowsservercore:1803 AS installer-env

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# set up environment
ENV NODE_VERSION 8.10.0
ENV NODE_DOWNLOAD_SHA 936ADA36CB6F09A5565571E15EB8006E45C5A513529C19E21D070ACF0E50321B

# Install node, bower, and git
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

# SDK image
FROM microsoft/dotnet:2.1-sdk
    
    
COPY --from=installer-env ["C:\\Program Files\\nodejs", "C:\\Program Files\\nodejs"]

# In order to set system PATH, ContainerAdministrator must be used
USER ContainerAdministrator 
RUN setx /M PATH "%PATH%;C:\Program Files\nodejs"
USER ContainerUser

RUN npm install -g gulp bower yarn