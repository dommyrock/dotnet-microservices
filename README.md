## /backend/Dockerfile explanation
```yaml
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY backend.csproj .
RUN dotnet restore
COPY . .
RUN dotnet publish -c release -o /app
```
This will perform the following steps sequentially when invoked:

1. Pull the mcr.microsoft.com/dotnet/sdk:6.0 image and name the image build.
2. Set the working directory within the image to /src.
3. Copy the file named backend.csproj found locally to the /src directory that was just created.
4. Calls dotnet restore on the project.
5. Copy everything in the local working directory to the image.
6. Calls dotnet publish on the project.

```yaml
FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /app
EXPOSE 80
EXPOSE 443
COPY --from=build /app .
ENTRYPOINT ["dotnet", "backend.dll"]
```

1. Pull the mcr.microsoft.com/dotnet/aspnet:6.0 image.
2. Set the working directory within the image to /app.
3. Exposes port 80 and 443.
4. Copy everything from the /app directory of the build image created above into the app directory of this image.
5. Sets the entrypoint of this image to dotnet and passes backend.dll as an argument.

6. Run this command to create image and tag the image with the name pizzabackend:
```
docker build -t pizzabackend .
```

7. To run the web API service, run the following command to start a new Docker container using the pizzabackend image and expose the service on port 5200:
```
docker run -it --rm -p 5200:80 --name pizzabackendcontainer pizzabackend
```


## Stop & remove running containers
```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```
[Remove-all-stopped-containers](https://docs.docker.com/engine/reference/commandline/rm/#remove-all-stopped-containers)

## /mslearn-dotnetmicroserices/docker-compose.yaml explanation

1. First it creates the frontend website, naming it pizza frontend. It tells Docker to build it, pointing to the Dockerfile found in the frontend folder. Then it sets an environment variable for the website: backendUrl=http://backend. Finally it opens a port and declares it depends on the backend service.
2. The backend service gets created next. It's named pizzabackend. It's built from the same Dockerfile you created in the previous exercise. The last command specifies which port to open.

--- 
To build the container images, open up a command prompt and from the same directory where the docker-compose.yml file is, run the following command:
```
docker-compose build
```
Then to start both the website and the web API, run this command:
```
docker-compose up
```
After a bit of output, the website and web API will be running. You should see something similar to the output below:
```
Attaching to docker-aspnet-pizza_backend_1, docker-aspnet-pizza_frontend_1
```

You can browse to: http://localhost:5902 to see the Contoso Pizza menu.

## Sample for the Build your first microservice in .NET learn module

Microservice applications are composed of small, independently versioned, and scalable customer-focused services that communicate with each other over standard protocols with well-defined interfaces. Each microservice typically encapsulates simple business logic, which you can scale out or in, test, deploy, and manage independently.  Smaller teams develop a microservice based on a customer scenario and use any technologies that they want to use. This module will teach you how to build your first microservice with .NET.

In order to run the code in this repo as a microservice, please complete the [Build your first microservice in .NET](https://docs.microsoft.com/learn/modules/dotnet-microservices).

### Steps to run

1. You will need to add the **Dockerfile** to the web API in the backend folder.
1. And add the **docker-compose.yml** file to the root folder.

Along with the necessary Docker commands for building an image and running Docker compose as well.

Check out the Learn module to find out all about [Building your first microservice in .NET](https://docs.microsoft.com/learn/modules/dotnet-microservices).


