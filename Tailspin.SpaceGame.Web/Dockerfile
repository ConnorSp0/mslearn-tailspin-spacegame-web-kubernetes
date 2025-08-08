# Base runtime image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

# Build image
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy csproj and restore
COPY ["Tailspin.SpaceGame.Web.csproj", "."]
RUN dotnet restore "Tailspin.SpaceGame.Web.csproj"

# Copy everything else and build
COPY . .
RUN dotnet build "Tailspin.SpaceGame.Web.csproj" -c Release -o /app/build

# Publish
FROM build AS publish
RUN dotnet publish "Tailspin.SpaceGame.Web.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Final runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Tailspin.SpaceGame.Web.dll"]
