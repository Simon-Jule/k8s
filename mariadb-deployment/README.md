#Create the Secret containing the MYSQL_ROOT_PASSWORD
kubectl apply -f mysql-secret.yaml

#Returns decoded secret string
kubectl get secret mariadb-root-password -o jsonpath='{.data.password}' | base64 --decode -

#Set up a regular database user and password
kubectl create secret generic mariadb-user-creds \
--from-literal=MYSQL_USER=kubeuser \
--from-literal=MYSQL_PASSWORD=kube-still-rocks

#Get username and password
kubectl get secret mariadb-user-creds -o jsonpath='{.data.MYSQL_USER}'
kubectl get secret mariadb-user-creds -o jsonpath='{.data.MYSQL_PASSWORD}' 

#Create a configMap
kubectl create configmap mariadb-config --from-file=max_allowed_packet.cnf

#Edit configMap
kubectl edit configmap mariadb-config

#Get configMap data
kubectl get configmap mariadb-config -o "jsonpath={.data['max_allowed_packet\.cnf']}" 

sources:
https://opensource.com/article/19/6/introduction-kubernetes-secrets-and-configmaps#:~:text=Kubernetes%20has%20two%20types%20of,or%20volumes%20or%20environment%20variables.
