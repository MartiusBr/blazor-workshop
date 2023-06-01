FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["BlazingPizza.Server/BlazingPizza.Server.csproj", "BlazingPizza.Server/"]
COPY ["BlazingPizza.Client/BlazingPizza.Client.csproj", "BlazingPizza.Client/"]
COPY ["BlazingComponents/BlazingComponents.csproj", "BlazingComponents/"]
COPY ["BlazingPizza.ComponentsLibrary/BlazingPizza.ComponentsLibrary.csproj", "BlazingPizza.ComponentsLibrary/"]
COPY ["BlazingPizza.Shared/BlazingPizza.Shared.csproj", "BlazingPizza.Shared/"]
RUN dotnet restore "BlazingPizza.Server/BlazingPizza.Server.csproj"
COPY . .
WORKDIR "/src/BlazingPizza.Server"
RUN dotnet build "BlazingPizza.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazingPizza.Server.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazingPizza.Server.dll"]