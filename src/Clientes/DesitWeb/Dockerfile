ARG NODE_IMAGE=node:8.11

FROM microsoft/dotnet:2.1-aspnetcore-runtime AS base
RUN apt-get update && \
    apt-get install -y wget && \
    apt-get install -y gnupg2 && \
    wget -qO- https://deb.nodesource.com/setup_8.x | bash - && \
    apt-get install -y build-essential nodejs
WORKDIR /app
EXPOSE 80

FROM microsoft/dotnet:2.1-sdk AS dotnet-build
WORKDIR /src

FROM ${NODE_IMAGE} as node-build
WORKDIR /web
COPY src/Clientes/DesitWeb .
RUN npm install
RUN npm run build:prod

FROM dotnet-build AS publish
WORKDIR /src/src/Clientes/DesitWeb/wwwroot
COPY --from=node-build /web/wwwroot .
WORKDIR /src
COPY . .
WORKDIR /src/src/Clientes/DesitWeb
RUN dotnet publish -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "DesitWeb.dll"]
