# TR Logic test task

## Wordpress application

Official build sources (Dockerfile and docker-entrypoint.sh) for tag `latest` has been used:
https://github.com/docker-library/wordpress/tree/666c5c06d7bc9d02c71fd48a74911248be6f5a5b/php5.6/apache

## Build image

    $ docker build -t <your_namespace>/wordpress ./wordpress
    
For example:

    $ docker build -t ujhgj/wordpress ./wordpress
    
## docker-compose

To bring up the project:

    $ docker-compose up -d

## Kubernetes

### Bringing up the application

Navigate to `kubernetes` folder:

    $ cd ./kubernetes
    
Create persistent volumes:
 
    $ kubectl create -f local-volumes.yaml
    
Create the Secret object from the following command:

    $ kubectl create secret generic mysql-pass --from-literal=password=password
    
Deploy Mysql and Wordpress app:

    $ kubectl create -f mysql-deployment.yaml
    $ kubectl create -f wordpress-deployment.yaml
    
If you use minikube to run Kubernetes you can get working url of the application using:

    $ minikube service wordpress --url
    
### Cleaning up
    
Run the following command to delete mysql Secret:
    
    $ kubectl delete secret mysql-pass
    
Run the following commands to delete all Deployments and Services:
    
    $ kubectl delete deployment -l app=wordpress
    $ kubectl delete service -l app=wordpress
    
Run the following commands to delete the PersistentVolumeClaims and the PersistentVolumes:
    
    $ kubectl delete pvc -l app=wordpress
    $ kubectl delete pv local-pv-1 local-pv-2