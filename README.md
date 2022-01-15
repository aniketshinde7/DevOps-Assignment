### Assumption
- The app has been tested successfully on minikube installed on my windows laptop.
- I have created persistence volume on C:/Users folder.
- Helm is initiated and tiller is running.

### Manifest file for Deployment using Helm
#### Webapp
- Deployment: configuration/helmcharts/webapp/templates/deployment.yaml
- service: configuration/helmcharts/webapp/templates/service.yaml
- values: configuration/helmcharts/webapp/values.yaml
#### mysql
- Deployment: configuration/helmcharts/mysql/templates/deployment.yaml
- service: configuration/helmcharts/mysql/templates/service.yaml
- secrets: configuration/helmcharts/mysql/templates/mysql-secret.yaml
- persistence storage: configuration/helmcharts/mysql/templates/pv.yaml
- persistence volume claim: configuration/helmcharts/mysql/templates/pvc.yaml
- values: configuration/helmcharts/mysql/values.yaml

## Deployment
#### Installation steps
- git clone https://github.com/vinga2805/relayr-test.git
- cd relayr-test/configurations/
- docker build -t aniketshinde7/my-app:latest . 
- docker login
- docker push aniketshinde7/my-app:latest
- vi helmcharts/webapp/values.yaml
- make changes in image1 as below 
- image1: aniketshinde7/my-app
- tag: latest
- cd helmcharts
- helm install --name webapp webapp
- minikune service webapp --url
- Initialize the database with sample schema
- curl http://$NODE_IP:$NODE_PORT/init
- Insert some sample data via postman
- curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "1", "user":"naruto"}' http://$NODE_IP:$NODE_PORT/users/add
- curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "2", "user":"itachi"}' http://$NODE_IP:$NODE_PORT/users/add
- curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "3", "user":"obito"}' http://$NODE_IP:$NODE_PORT/users/add

- Access the data
- curl http://$NODE_IP:$NODE_PORT/users/1
- The second time you access the data, it appends '(c)' indicating that it is pulled from the Redis cache
- curl http://$NODE_IP:$NODE_PORT/users/1
