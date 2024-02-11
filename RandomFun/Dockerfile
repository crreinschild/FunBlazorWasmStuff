FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["RandomFun/RandomFun.csproj", "RandomFun/"]
RUN dotnet restore "RandomFun/RandomFun.csproj"
COPY . .
WORKDIR "/src/RandomFun"
RUN dotnet build "RandomFun.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "RandomFun.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM nginx:alpine AS final
USER $APP_UID
WORKDIR /var/www/web
COPY --from=publish /app/publish/wwwroot .
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80