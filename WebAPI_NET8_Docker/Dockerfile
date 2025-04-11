#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081
ENV DB_CONNECTION_STRING="Server=sqlserverdocker;Database=ApiCustomersDockerSql2;User Id=sa;Password=MyPassword*1234;TrustServerCertificate=True;"
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["WebAPI_NET8_Docker/WebAPI_NET8_Docker.csproj", "WebAPI_NET8_Docker/"]
RUN dotnet restore "./WebAPI_NET8_Docker/WebAPI_NET8_Docker.csproj"
COPY . .
WORKDIR "/src/WebAPI_NET8_Docker"
RUN dotnet build "./WebAPI_NET8_Docker.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./WebAPI_NET8_Docker.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebAPI_NET8_Docker.dll"]