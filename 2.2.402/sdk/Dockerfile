# escape=`
FROM netpotential/dotnet:base-sdk-latest AS installer-env

# SDK image
FROM mcr.microsoft.com/dotnet/core/sdk:2.2.402-nanoserver-1803

MAINTAINER dealdiane@netpotential.co.nz

ENV NODE_VERSION 10.16.3
ENV NODE_DOWNLOAD_SHA 19AA47DE7C5950D7BD71A1E878013B98D93871CC311D7185F5472E6D3F633146

COPY --from=installer-env ["C:\\Program Files\\git", "C:\\Program Files\\git"]

# In order to set system PATH, ContainerAdministrator must be used
USER ContainerAdministrator 
RUN setx /M PATH "%PATH%;C:\Program Files\nodejs;C:\Program Files\git\cmd"
USER ContainerUser

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

# Install package managers
RUN npm install -g gulp bower yarn

# Trigger first run experience by running arbitrary cmd to populate local package cache
RUN dotnet help