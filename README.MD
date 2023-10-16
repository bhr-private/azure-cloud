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

docker build -t petstoreproductservice .


docker cp applicationinsights.json brave_cohen:/app/applicationinsights.json
docker cp applicationinsights-agent-3.4.12.jar brave_cohen:/app/applicationinsights-agent-3.4.12.jar
docker cp petstoreproductservice.jar brave_cohen:/app/petstoreproductservice.jar

docker run --rm --net petstorebridge --name petstorepetservice -p 8081:8081 -e PETSTOREPETSERVICE_SERVER_PORT=8081 -d nifty_yonath:version1
docker run --rm --net petstorebridge --name petstoreproductservice -p 8082:8082 -e PETSTOREPRODUCTSERVICE_SERVER_PORT=8082 -d brave_cohen:version1

/Users/bhr/Library/Containers/com.docker.docker/Data

