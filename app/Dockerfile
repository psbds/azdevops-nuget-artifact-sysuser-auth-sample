#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src

# Private Nuget Configuration
ARG nuget_name
ARG nuget_server
ARG nuget_user
ARG nuget_password
RUN dotnet nuget add source $nuget_server -n $nuget_name -u $nuget_user -p "$nuget_password" --store-password-in-clear-text

COPY ["SampleApp.csproj", ""]
RUN dotnet restore "./SampleApp.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "SampleApp.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SampleApp.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SampleApp.dll"]