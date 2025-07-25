FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src

# Copy everything first
COPY . .

# Restore and build
RUN dotnet restore
RUN dotnet publish -c Release -o /app/publish /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS runtime
WORKDIR /app
EXPOSE 10000
ENV ASPNETCORE_URLS=http://+:10000

# Copy published app
COPY --from=build /app/publish .

# Create Umbraco directories
RUN mkdir -p /app/umbraco/Data
RUN chmod 755 /app/umbraco/Data

ENTRYPOINT ["dotnet", "MyProject.dll"]