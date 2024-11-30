# Use the official image as a base for the runtime environment
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Use the SDK image to build the application
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src

# Copy the .csproj file first to ensure Docker caches the restore layer
COPY ["HelloRealWorldAPI.csproj", "HelloRealWorldAPI/"]

# Restore dependencies (via dotnet restore)
RUN dotnet restore "HelloRealWorldAPI/HelloRealWorldAPI.csproj"

# Copy the rest of the application code into the container
COPY . .

# Set the working directory to where the .csproj is located
WORKDIR "/src/HelloRealWorldAPI"

# Build the application in Release mode and output to /app/build
RUN dotnet build "HelloRealWorldAPI.csproj" -c Release -o /app/build

# Publish the application (for production)
FROM build AS publish
RUN dotnet publish "HelloRealWorldAPI.csproj" -c Release -o /app/publish

# Define the base image and copy the published app from the publish stage
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Set the entry point to run the application
ENTRYPOINT ["dotnet", "HelloRealWorldAPI.dll"]
