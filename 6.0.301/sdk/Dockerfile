# escape=`
FROM netpotential/dotnet:base-sdk-latest AS installer-env

# SDK image
FROM mcr.microsoft.com/dotnet/sdk:6.0.301-windowsservercore-ltsc2019

MAINTAINER dealdiane@netpotential.co.nz

COPY --from=installer-env ["C:\\Program Files\\nodejs", "C:\\Program Files\\nodejs"]
COPY --from=installer-env ["C:\\Program Files\\git", "C:\\Program Files\\git"]

# Run as admin to install
USER ContainerAdministrator 

RUN setx /M PATH "%PATH%;C:\Program Files\nodejs;C:\Program Files\git\cmd;C:\Python3"

# Install node build tools
# Normally should be npm install --global --production windows-build-tools but currently hangs and doesn't finish
RUN Invoke-WebRequest https://aka.ms/vs/15/release/vs_buildtools.exe -outfile vs_BuildTools.exe; \
    .\vs_BuildTools.exe --add Microsoft.VisualStudio.Workload.VCTools --quiet --norestart --nocache --wait --includeRecommended; \
    if ($err = dir $Env:TEMP -Filter dd_setup_*_errors.log | where Length -gt 0 | Get-Content) { throw $err }; \
    Remove-Item vs_BuildTools.exe

# Install package managers
RUN npm install --global gulp bower yarn

# Install and compile sass
RUN npm install --global node-sass `
    && npm rebuild node-sass

# Stay as admin to minimise access issues
# USER ContainerUser

# Trigger first run experience by running arbitrary cmd to populate local package cache
RUN dotnet help