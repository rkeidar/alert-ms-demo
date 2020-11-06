FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY ["src/AlertService/test/test.api/test.api.csproj", "src/AlertService/test/test.api/"]
RUN dotnet restore "src/AlertService/test/test.api/test.api.csproj"
COPY . .
WORKDIR "/src/src/AlertService/test/test.api"
RUN dotnet build "test.api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "test.api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "test.api.dll"]