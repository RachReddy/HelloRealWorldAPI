# Use the official .NET Core SDK as a parent image
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /app

# Copy the project file and restore any dependencies (use .csproj for the project name)
COPY *.csproj ./
RUN dotnet restore

# Copy the rest of the application code into the container
COPY . .

# Publish the application 
RUN dotnet publish -c Release -o out

# Build the runtime image
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS runtime
WORKDIR /app
COPY --from=build /app/out ./

#expose the port your application will run on
EXPOSE 80

# Set the entry point to run the application
ENTRYPOINT ["dotnet", "HelloRealWorldAPI.dll"]
