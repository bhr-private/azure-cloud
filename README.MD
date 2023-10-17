# Here you will find various [Azure](https://ms.portal.azure.com/) demos & learning guides that are meant to help you build awesome stuff on Azure.

## [View Azure Pet Store Learning Guides](https://github.com/chtrembl/azure-cloud/tree/main/petstore)

Extensive Azure learning guides for a hypothetical Azure Pet Store walking you thorugh many Azure services and development scenarios using pre built web applications and microdervices.

> You can view the live deployment here https://azurepetstore.com


# GIT
git remote add upstream git@github.com:chtrembl/azure-cloud.git
git fetch upstream
git checkout main
git merge upstream/main





# BUILD DOCKER IMAGES

run mvn manually, copy .jar files ( petstore*, annd applicationinsights ) to ./app, then run docker build

FROM openjdk:8-jre-alpine

WORKDIR /app

# RUN apk update && apk add curl 
# CMD /bin/bash
# RUN echo "Downloading applicationinsights-agent-3.4.12.jar"
# RUN curl "https://repo1.maven.org/maven2/com/microsoft/azure/applicationinsights-agent/3.4.12/applicationinsights-agent-3.4.12.jar" -o /app/applicationinsights-agent-3.4.12.jar

# COPY --from=build /build/target/*.jar /app/petstoreorderservice.jar
# COPY applicationinsights.json /app/applicationinsights.json
EXPOSE 8080
ENTRYPOINT ["java", "-javaagent:applicationinsights-agent-3.4.12.jar", "-jar","petstoreorderservice.jar"]




docker build -t petstoreproductservice .

docker cp applicationinsights.json mystifying_cerf:/app/applicationinsights.json
docker cp applicationinsights-agent-3.4.12.jar mystifying_cerf:/app/applicationinsights-agent-3.4.12.jar
docker cp petstoreorderservice.jar mystifying_cerf:/app/petstoreorderservice.jar

docker commit 1cb95de02564 brave_cohen:version1

docker image tag 897f2b82d904 petstorepetservice:v1
docker image tag 5bfe32f1af86 petstoreproductservice:v1
docker image tag e855ed329b1f petstoreorderservice:v1



docker run --rm --net petstorebridge --name petstorepetservice -p 8081:8081 -e PETSTOREPETSERVICE_SERVER_PORT=8081 -d petstorepetservice:v1
docker run --rm --net petstorebridge --name petstoreproductservice -p 8082:8082 -e PETSTOREPRODUCTSERVICE_SERVER_PORT=8082 -d petstoreproductservice:v1
docker run --rm --net petstorebridge --name petstoreorderservice -p 8083:8083 -e PETSTOREORDERSERVICE_SERVER_PORT=8083 -d petstoreorderservice:v1



docker ps -q | xargs -n1 docker inspect --format '{{.Name}} {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'
/petstoreorderservice 172.18.0.4
/petstoreproductservice 172.18.0.3
/petstorepetservice 172.18.0.2


docker run --rm --net petstorebridge --name petstoreapp -p 8080:8080 -e PETSTOREAPP_SERVER_PORT=8080 -e PETSTOREPETSERVICE_URL=http://172.18.0.2:8081 -e PETSTOREPRODUCTSERVICE_URL=http://172.18.0.3:8082 -e PETSTOREORDERSERVICE_URL=http://172.18.0.4:8083 -d petstoreapp:latest

## Mac image location
/Users/bhr/Library/Containers/com.docker.docker/Data


# ACR
create admin credentials
az acr login --name bhrazurepetstorecr

docker image tag petstoreapp:latest bhrazurepetstorecr.azurecr.io/petstoreapp:latest 
docker push bhrazurepetstorecr.azurecr.io/petstoreapp:latest

docker image tag petstorepetservice:v1 bhrazurepetstorecr.azurecr.io/petstorepetservice:latest
docker push bhrazurepetstorecr.azurecr.io/petstorepetservice:latest

docker image tag petstoreproductservice:v1 bhrazurepetstorecr.azurecr.io/petstoreproductservice:latest
docker push bhrazurepetstorecr.azurecr.io/petstoreproductservice:latest

docker image tag petstoreorderservice:v1 bhrazurepetstorecr.azurecr.io/petstoreorderservice:latest
docker push bhrazurepetstorecr.azurecr.io/petstoreorderservice:latest


no response for 8080 ( 80 OK )
az webapp config appsettings list --name bhrazurepetstore --resource-group learn --subscription 8db9a596-7fac-4304-8339-d7e866ddeaeb
 {
    "name": "WEBSITES_PORT",
    "slotSetting": false,
    "value": "8080"
  }

  





