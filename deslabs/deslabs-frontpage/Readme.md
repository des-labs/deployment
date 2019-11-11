Deploy the DES Labs front page
--------------------------------

### Build Helm Docker image (optional)

If you do not have Helm installed, you can use a disposable Docker image instead of installing it locally to use for the steps below. Clone the git repo and build the local image:
```
git clone https://github.com/manning-ncsa/helm-docker
docker build -t helm:v2.15.1 helm-docker
```

### Create the default backend

Clone the `des-labs/deployment` repo and generate the Kubernetes config file for the default backend service:
```
git clone --recursive --branch desapps https://github.com/des-labs/deployment des-labs/deployment
cd des-labs/deployment/ingress
docker run --rm -v $(pwd):/apps helm:v2.15.1 template default-backend > deploy_default_backend.yaml
```
Apply the YAML file to the cluster configuration:
```
kubectl apply -f deploy_default_backend.yaml
```

### Create the NGINX ingress controller

Clone the `des-labs/deployment` repo and generate the Kubernetes config file for the ingress controller:
```
git clone --recursive --branch desapps https://github.com/des-labs/deployment des-labs/deployment
cd des-labs/deployment/ingress
docker run --rm -v $(pwd):/apps helm:v2.15.1 template ingress-controller > deploy_nginx_ingress_controller.yaml
```
Apply the YAML file to the cluster configuration:
```
kubectl apply -f deploy_nginx_ingress_controller.yaml
```
### Build and deploy the DES Labs front page

Start a local Docker image registry server:
```
docker run -d -p 5000:5000 --name registry registry:2
```
Clone the deployment repo
```
git clone --recursive --branch desapps https://github.com/des-labs/deployment des-labs/deployment
```
Build the Docker image defined by `des-labs/deployment/deslabs/deslabs-frontpage/Dockerfile`. During the build process, a separate git repo [`des-labs/frontpage`](https://github.com/des-labs/frontpage) is cloned so that the web page files in `des-labs/frontpage:/public_html` can be copied into the `frontpage` image.
```
docker build -t localhost:5000/deslabs-frontpage des-labs/deployment/deslabs/deslabs-frontpage
```
Push the image to the local registry:
```
docker push localhost:5000/deslabs-frontpage
```
Render the Kubernetes config file using Helm and then apply it to the cluster.
```
cd des-labs/deployment/deslabs/deslabs-frontpage
docker run --rm -v $(pwd):/apps helm:v2.15.1 template frontpage > deploy_deslabs_frontpage.yaml
kubectl apply -f deploy_deslabs_frontpage.yaml
```
