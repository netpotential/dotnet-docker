# escape=`
FROM netpotential/dotnet:base-sdk-latest AS installer-env

# SDK image
FROM microsoft/windowsservercore:1803

MAINTAINER dealdiane@netpotential.co.nz
    
COPY --from=installer-env ["C:\\Program Files\\nodejs", "C:\\Program Files\\nodejs"]
COPY --from=installer-env ["C:\\Program Files\\git", "C:\\Program Files\\git"]

# Install .NET Core SDK
ENV DOTNET_SDK_VERSION 1.1.11
ENV DOTNET_SDK_DOWNLOAD_URL https://dotnetcli.blob.core.windows.net/dotnet/Sdk/$DOTNET_SDK_VERSION/dotnet-dev-win-x64.$DOTNET_SDK_VERSION.zip

RUN Invoke-WebRequest $Env:DOTNET_SDK_DOWNLOAD_URL -OutFile dotnet.zip; `
    Expand-Archive dotnet.zip -DestinationPath $Env:ProgramFiles\dotnet; `
    Remove-Item -Force dotnet.zip

# In order to set system PATH, ContainerAdministrator must be used
USER ContainerAdministrator 
RUN setx /M PATH $($Env:PATH + ';' + $Env:ProgramFiles + '\dotnet')
RUN setx /M PATH "%PATH%;C:\Program Files\nodejs;C:\Program Files\git\cmd"
USER ContainerUser

# Install package managers
RUN npm install -g gulp bower yarn

# Trigger the population of the local package cache
ENV NUGET_XMLDOC_MODE skip
RUN New-Item -Type Directory warmup; `
    cd warmup; `
    dotnet new; `
    cd ..; `
    Remove-Item -Force -Recurse warmup