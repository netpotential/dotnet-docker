# escape=`
FROM netpotential/dotnet:base-sdk-latest AS installer-env

# SDK image
FROM mcr.microsoft.com/dotnet/sdk:5.0.301-windowsservercore-ltsc2019

MAINTAINER dealdiane@netpotential.co.nz

COPY --from=installer-env ["C:\\Program Files\\nodejs", "C:\\Program Files\\nodejs"]
COPY --from=installer-env ["C:\\Program Files\\git", "C:\\Program Files\\git"]

# Run as admin to install
USER ContainerAdministrator 

RUN setx /M PATH "%PATH%;C:\Program Files\nodejs;C:\Program Files\git\cmd;C:\Python3"

# Install chocolatey
ENV chocolateyUseWindowsCompression false

RUN powershell -Command " `
    [System.Net.ServicePointManager]::SecurityProtocol = 3072; `
    Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1')); `
    &choco feature disable --name showDownloadProgress"

# Install package managers (and Python for building native node modules)
RUN npm install --global gulp bower `
    && choco install yarn -y `
    && choco install python3 -y --params "/InstallDir:C:\Python3" `
    && setx /M PATH "%PATH%;C:\Python3;%PROGRAMFILES(X86)%\Yarn\bin"

# Install node build tools
# Normally should be npm install --global --production windows-build-tools but currently hangs and doesn't finish
ADD https://aka.ms/vs/15/release/vs_buildtools.exe C:\\vs15_buildtools.exe

RUN C:\\vs15_buildtools.exe --wait --quiet --norestart --nocache --add Microsoft.VisualStudio.Workload.VCTools --includeRecommended `
    && powershell -Command "Remove-Item -Force C:\vs15_buildtools.exe"

RUN npm install --global node-sass `
    && npm rebuild node-sass

# Stay as admin to minimise access issues
# USER ContainerUser

# Trigger first run experience by running arbitrary cmd to populate local package cache
RUN dotnet help