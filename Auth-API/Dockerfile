#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Auth-API/Auth-API.csproj", "Auth-API/"]
COPY ["Auth-Data/Auth-Data.csproj", "Auth-Data/"]
RUN dotnet restore "Auth-API/Auth-API.csproj"
COPY . .
WORKDIR "/src/Auth-API"
RUN dotnet build "Auth-API.csproj" -c Release -o /app/build
FROM build as test
WORKDIR "/src/Auth-Test/"

FROM build AS publish
RUN dotnet publish "Auth-API.csproj" -c Release -o /app/publish /p:UseAppHost=false


FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Auth-API.dll"]