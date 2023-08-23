#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Auth/IssueTracker.Auth.API/IssueTracker.Auth.API.csproj", "Auth/IssueTracker.Auth.API/"]
RUN dotnet restore "Auth/IssueTracker.Auth.API/IssueTracker.Auth.API.csproj"
COPY . .
WORKDIR "/src/Auth/IssueTracker.Auth.API"
RUN dotnet build "IssueTracker.Auth.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "IssueTracker.Auth.API.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "IssueTracker.Auth.API.dll"]