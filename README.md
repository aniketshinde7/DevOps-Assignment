### Assumption
- The app has been tested successfully on minikube installed on my windows laptop.
- I have created persistence volume on C:/Users folder.
- Helm is initiated.

### Manifest file for Deployment using Helm
#### my-app
- Deployment: configuration/helmcharts/my-app/templates/deployment.yaml
- service: configuration/helmcharts/my-app/templates/service.yaml
- values: configuration/helmcharts/my-app/values.yaml
#### mysql
- Deployment: configuration/helmcharts/mysql/templates/deployment.yaml
- service: configuration/helmcharts/mysql/templates/service.yaml
- secrets: configuration/helmcharts/mysql/templates/mysql-secret.yaml
- persistence storage: configuration/helmcharts/mysql/templates/pv.yaml
- persistence volume claim: configuration/helmcharts/mysql/templates/pvc.yaml
- values: configuration/helmcharts/mysql/values.yaml

## Deployment
#### Installation steps
- git clone https://github.com/aniketshinde7/DevOps-Assignment.git
- cd DevOps-Assignment/configurations/
- docker build -t aniketshinde7/my-app:latest . 
- docker login
- docker push aniketshinde7/my-app:latest
- vi helmcharts/my-app/values.yaml
- make changes in image1 as below 
- image1: aniketshinde7/my-app
- tag: latest
- cd helmcharts
- helm install --name mysql mysql
- helm install --name my-app my-app
- minikune service my-app --url
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
