FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src

# Copy project file - note the path from repo root perspective
COPY ["MyProject/MyProject.csproj", "./"]
RUN dotnet restore "MyProject.csproj"

# Copy everything from MyProject folder
COPY MyProject/ .
RUN dotnet publish "MyProject.csproj" -c Release -o /app/publish /p:UseAppHost=false

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