# escape=`
FROM netpotential/dotnet:base-sdk-latest AS installer-env

# SDK image
FROM mcr.microsoft.com/dotnet/core/sdk:5.0.301-windowsservercore-ltsc2019

MAINTAINER dealdiane@netpotential.co.nz

COPY --from=installer-env ["C:\\Program Files\\nodejs", "C:\\Program Files\\nodejs"]
COPY --from=installer-env ["C:\\Program Files\\git", "C:\\Program Files\\git"]

# In order to set system PATH, ContainerAdministrator must be used
USER ContainerAdministrator 
RUN setx /M PATH "%PATH%;C:\Program Files\nodejs;C:\Program Files\git\cmd"
USER ContainerUser

# Install chocolatey
ENV chocolateyUseWindowsCompression false

RUN [System.Net.ServicePointManager]::SecurityProtocol = 3072; \
    Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1')); \
    &choco feature disable --name showDownloadProgress"

# Install package managers
RUN npm install --global gulp bower \
    choco install yarn -y

# Install node build tools
# Normally should be npm install --global --production windows-build-tools but currently hangs and doesn't finish
ADD https://aka.ms/vs/15/release/vs_buildtools.exe C:\\vs15_buildtools.exe

RUN C:\\vs15_buildtools.exe --wait --quiet --norestart --nocache --add Microsoft.VisualStudio.Workload.VCTools --includeRecommended \
    Remove-Item -Force C:\vs15_buildtools.exe

# Compile sass
RUN npm rebuild node-sass

# Trigger first run experience by running arbitrary cmd to populate local package cache
RUN dotnet help