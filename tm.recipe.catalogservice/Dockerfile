FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 6000

ENV ASPNETCORE_URLS=http://+:6000

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["tm.recipe.catalogservice/tm.recipe.catalogservice.csproj", "tm.recipe.catalogservice/"]
RUN dotnet restore "tm.recipe.catalogservice/tm.recipe.catalogservice.csproj"
COPY . .
WORKDIR "/src/tm.recipe.catalogservice"
RUN dotnet build "tm.recipe.catalogservice.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "tm.recipe.catalogservice.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "tm.recipe.catalogservice.dll"]
