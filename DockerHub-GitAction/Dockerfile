#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["DockerHub-GitAction.csproj", "DockerHub-GitAction/"]
RUN dotnet restore "DockerHub-GitAction/DockerHub-GitAction.csproj"
COPY . .
WORKDIR "/src/DockerHub-GitAction"
COPY . .
RUN dotnet build "DockerHub-GitAction.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DockerHub-GitAction.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DockerHub-GitAction.dll"]

