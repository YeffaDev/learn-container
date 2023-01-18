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

# Check del context
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
