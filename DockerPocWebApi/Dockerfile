#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src

COPY ["DockerPocWebApi.csproj", "./"]
#set proxy here if needed
RUN dotnet restore "./DockerPocWebApi.csproj"
COPY . .
WORKDIR "/src/"
RUN dotnet build "DockerPocWebApi.csproj" -c Release -o /app/build
#
FROM build AS publish
RUN dotnet publish "DockerPocWebApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_URLS=http://+:5000
ENV ASPNETCORE_ENVIRONMENT=Development
ENTRYPOINT ["dotnet", "DockerPocWebApi.dll"]