#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["AdventureWorks.Web/AdventureWorks.Web.csproj", "AdventureWorks.Web/"]
COPY ["AdventureWorks.Context/AdventureWorks.Context.csproj", "AdventureWorks.Context/"]
COPY ["AdventureWorks.Models/AdventureWorks.Models.csproj", "AdventureWorks.Models/"]
RUN dotnet restore "AdventureWorks.Web/AdventureWorks.Web.csproj"
COPY . .
WORKDIR "/src/AdventureWorks.Web"
RUN dotnet build "AdventureWorks.Web.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AdventureWorks.Web.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AdventureWorks.Web.dll"]