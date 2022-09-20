#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Forecast.API/Forecast.API.csproj", "Forecast.API/"]
RUN dotnet restore "Forecast.API/Forecast.API.csproj"
COPY . .
WORKDIR "/src/Forecast.API"
RUN dotnet build "Forecast.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Forecast.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Forecast.API.dll"]
