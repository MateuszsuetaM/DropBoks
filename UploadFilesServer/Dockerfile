FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
# FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
#FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
#FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build

WORKDIR /DockerSource

# Copy csproj and restore as distinct layers
COPY *.sln .
COPY UploadFilesServer/UploadFilesServer.csproj ./UploadFilesServer/
RUN dotnet restore

# Copy everything else and build website
COPY UploadFilesServer/. /UploadFilesServer/
WORKDIR /DockerSource/UploadFilesServer
RUN dotnet publish -c release -o /DockerOutput/FilesServer --no-restore
#RUN dotnet publish -c release -o /DockerOutput/UploadFilesServer --no-restore

# Final stage

FROM mcr.microsoft.com/dotnet/aspnet:5.0
WORKDIR /DockerOutput/FilesServer
COPY --from=build /DockerOutput/FilesServer ./
ENTRYPOINT ["dotnet", "UploadFilesServer.dll"]