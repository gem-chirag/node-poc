"# node-poc" 

docker image save -o node-poc-image.tar localhost:8082/node-poc-image:latest
minikube image load node-poc-image.tar