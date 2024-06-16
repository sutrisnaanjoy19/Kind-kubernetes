kind create cluster --name kind-host

helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/

helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard

kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443

kubectl apply -f hostmachine-rbac.yaml

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm install kind-nginx ingress-nginx/ingress-nginx --values kind-nginx.yaml -n nginx

KUBECONFIG=$(kind get kubeconfig-path) kubectl config view | grep server

For a loadbalancer[external IPs] to work we need an external ip which we cannot provide which is for GKE OR EKS environment where GCP or AWS provide the public ip.

In our case we will be using the node internal ip and loadbalancers forwarded TCP port

Now our ingress of 2048 us working on path http://172.18.0.3:31087/test-2048

Note: liveness and rediness will not work with HttpGet because the application must listen to the path to make this work we need to change in application

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm install kind-prometheus prometheus-community/kube-prometheus-stack -n monitoring

kubectl port-forward prometheus-kind-prometheus-kube-prome-prometheus-0 9090 -n monitoring

k port-forward kind-prometheus-grafana-659fcf7cc6-w86sx 3000 -n monitoring

kubectl get secret --namespace monitoring kind-prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
