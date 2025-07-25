FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 10000
ENV ASPNETCORE_URLS=http://+:10000

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src
COPY ["MyProject.csproj", "."]
RUN dotnet restore "./MyProject.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "MyProject.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MyProject.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Create directory for Umbraco data
RUN mkdir -p /app/umbraco/Data
RUN chmod 755 /app/umbraco/Data

ENTRYPOINT ["dotnet", "MyProject.dll"]