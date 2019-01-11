## Create Asp.Net Core Project

`dotnet new mvc -o DockerCoreApp`

## Create dockerfile

`type nul>dockerfile`

## dockerfile content

```
FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src
COPY ["DockerCoreApp.csproj", "DockerCoreApp.csproj"]
RUN dotnet restore "DockerCoreApp.csproj"
COPY . .
RUN dotnet build "DockerCoreApp.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "DockerCoreApp.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "DockerCoreApp.dll"]
```
## Create docker image

`docker build -t dockercoreapp:v1 .`

## check generated image

`docker image ls`

## run docker container

`docker run -it -p 8080:80 --name dockercoreapp1 dockercoreapp:v1`

## Check logs

`docker exec -it dockercoreapp3 /bin/bash`
`more app-20190111.txt`

## mount 

`docker run -it -p 8080:80 -v D:/Test:/app/Logs --name dockercoreapp1 dockercoreapp:v1`

`docker exec -it dockercoreapp /bin/bash`