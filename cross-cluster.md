# Cross Cluster Minikube Setup

start --apiserver-ips=192.168.122.56 --profile guest-cluster

create a RBAC for host-machine service account

kubect apply -f hostmachine-rbac.yaml

kubetl create token <serviceaccount: host-machine>

scp /home/sushi/.minikube/ca.crt sushi@192.168.122.1:/home/sushi/.minikube/profiles/guest-cluster-ca.crt

scp /home/sushi/.minikube/profiles/guest-cluster/client.crt sushi@192.168.122.1:/home/sushi/.minikube/profiles/guest-cluster-client.crt

scp /home/sushi/.minikube/profiles/guest-cluster/client.key sushi@192.168.122.1:/home/sushi/.minikube/profiles/guest-cluster-client.key
