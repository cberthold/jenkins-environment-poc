# install docker desktop with k8s
----

# install kubernetes-helm
choco install kubernetes-helm

# install tiller
helm init

# verify tiller running tiller-deploy pod
kubectl get pods --all-namespaces

# install rbac service account
kubectl apply -f rbac_helm.yaml

# install tiller
helm init --service-account tiller

# add helm repo for bitnami
helm repo add bitnami https://charts.bitnami.com/bitnami

# install jenkins via helm
helm install --name banking-ant --set jenkinsUsername=admin,jenkinsPassword=admin123!,service.port=30080,service.httpsPort=30443 bitnami/jenkins

NOTES:

** Please be patient while the chart is being deployed **

1. Get the Jenkins URL by running:

** Please ensure an external IP is associated to the banking-ant-jenkins service before proceeding **
** Watch the status using: kubectl get svc --namespace default -w banking-ant-jenkins **

  export SERVICE_IP=$(kubectl get svc --namespace default banking-ant-jenkins --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}")
  echo "Jenkins URL: http://$SERVICE_IP/"

2. Login with the following credentials

  echo Username: user
  echo Password: $(kubectl get secret --namespace default banking-ant-jenkins -o jsonpath="{.data.jenkins-password}" | base64 --decode)
