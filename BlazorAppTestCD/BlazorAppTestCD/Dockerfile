#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["BlazorAppTestCD/BlazorAppTestCD/BlazorAppTestCD.csproj", "BlazorAppTestCD/BlazorAppTestCD/"]
COPY ["BlazorAppTestCD/BlazorAppTestCD.Client/BlazorAppTestCD.Client.csproj", "BlazorAppTestCD/BlazorAppTestCD.Client/"]
RUN dotnet restore "./BlazorAppTestCD/BlazorAppTestCD/./BlazorAppTestCD.csproj"
COPY . .
WORKDIR "/src/BlazorAppTestCD/BlazorAppTestCD"
RUN dotnet build "./BlazorAppTestCD.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./BlazorAppTestCD.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorAppTestCD.dll"]