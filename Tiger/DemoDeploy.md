# Crea alias kubectl
alias k=kubectl

# Crea scorciatoia per creazione file yaml
export do="--dry-run=client -o yaml"

# Check contexts
k config get-contexts

# Creazione nuovo namespace
k create namespace project-tiger

# Modifica namespace di default nel context
k config set-context --current --namespace="project-tiger"

# Check della modifica
k config get-contexts

# Creazione di un nuovo deployment in yaml
k -n project-tiger create deployment --image=nginx nginx-deploy-1 $do 
k -n project-tiger create deployment --image=nginx nginx-deploy-1 $do > nginx-deploy-1.yaml

# Creazione deployment
k apply -f nginx-deploy-1.yaml

# Check se sono stati creati degli oggetti
k get all

# Scalare il numero di repliche nel deployment
k -n project-tiger scale deployment nginx-deploy-1 --replicas=3

# Check delle repliche
k get pod --watch
k get replicaset

# Esposizione servizio sulla porta 80
k expose deployment nginx-deploy-1 --type=NodePort --port=80 --target-port=80

# Check servizio
k get svc

# Port-forward in localhost sulla porta 8080
k port-forward svc/nginx-deploy-1 8080:80

# Eliminare deployment
k delete -f nginx-deploy-1.yaml

# Eliminare service
k delete svc/nginx-deploy-1

#############################

k apply -f time-app.yaml
k apply -f hello-app.yaml
k apply -f api-gtw-demo.yaml

k -n api-gtw-demo port-forward svc/api-gtw-demo 8080:80

#############################

# install helm

curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null

sudo apt-get install apt-transport-https --yes

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list

sudo apt-get update

sudo apt-get install helm


# install ingress via helm

git clone https://github.com/nginxinc/kubernetes-ingress.git --branch v3.0.0

cd kubernetes-ingress/deployments/helm-chart

helm repo add nginx-stable https://helm.nginx.com/stable

helm repo update

helm install nginxing nginx-stable/nginx-ingress --create-namespace --namespace nginx-ingress


