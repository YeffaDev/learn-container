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

# Creazione pod
k -n project-tiger run nginx-pod-1 --image=nginx

# Creazione pod tramite yaml
k -n project-tiger run nginx-pod-1 --image=nginx $do > nginx-pod-1.yaml

# Check pod creato
k -n project-tiger get pod

k -n project-tiger describe pod

# Check log pod

k -n project-tiger logs pod/nginx-pod-1

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
k -n project-tiger get pod --watch

k -n project-tiger get replicaset

# Riavvio deployment

k -n project-tiger rollout restart deployment nginx-deploy-1 

# Esposizione servizio sulla porta 80
k -n project-tiger expose deployment nginx-deploy-1 --type=NodePort --port=80 --target-port=80

# Check servizio
k -n project-tiger get svc

# Port-forward in localhost sulla porta 8080
k -n project-tiger port-forward svc/nginx-deploy-1 8080:80

# Eliminare pod
k -n project-tiger delete nginx-pod-1

# Eliminare deployment
k -n project-tiger delete -f nginx-deploy-1.yaml

# Eliminare service
k -n project-tiger delete svc/nginx-deploy-1

# Eliminare namespace
k delete ns project-tiger
