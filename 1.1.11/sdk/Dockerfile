# escape=`
FROM netpotential/dotnet:base-sdk-latest AS installer-env

# SDK image
FROM microsoft/dotnet:1.1.10-sdk-1.1.11

MAINTAINER dealdiane@netpotential.co.nz
    
COPY --from=installer-env ["C:\\Program Files\\nodejs", "C:\\Program Files\\nodejs"]
COPY --from=installer-env ["C:\\Program Files\\git", "C:\\Program Files\\git"]

# In order to set system PATH, ContainerAdministrator must be used
USER ContainerAdministrator 
RUN setx /M PATH "%PATH%;C:\Program Files\nodejs;C:\Program Files\git\cmd"
USER ContainerUser

# Install package managers
RUN npm install -g gulp bower yarn

# Trigger first run experience by running arbitrary cmd to populate local package cache
RUN dotnet help