FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

# remover se não usar .editorconfig
ENV PATH="$PATH:/root/.dotnet/tools"
RUN dotnet tool install -g dotnet-format

WORKDIR /src
COPY . .

RUN dotnet restore
RUN dotnet build -c Release --no-restore
RUN dotnet test -c Release --no-build

# remover se não usar .editorconfig
RUN dotnet format --verify-no-changes

RUN dotnet publish "src/MyProject.csproj" -c Release --no-build -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "MyProject.dll"]