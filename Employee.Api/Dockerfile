#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Employee.Api/Employee.Api.csproj", "Employee.Api/"]
COPY ["Employee.Core/Employee.Core.csproj", "Employee.Core/"]
COPY ["Employee.Data/Employee.Data.csproj", "Employee.Data/"]
COPY ["Employee.Service/Employee.Service.csproj", "Employee.Service/"]
RUN dotnet restore "Employee.Api/Employee.Api.csproj"
COPY . .
WORKDIR "/src/Employee.Api"
RUN dotnet build "Employee.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Employee.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Employee.Api.dll"]