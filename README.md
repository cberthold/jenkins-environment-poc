# install docker desktop with k8s
----

# install kubernetes-helm
choco install kubernetes-helm

# install rbac service account for tiller
kubectl apply -f rbac_helm.yaml

# install tiller with rbac service account
helm init --service-account tiller

# verify tiller running tiller-deploy pod
kubectl get pods --all-namespaces

# add helm repo for bitnami
helm repo add bitnami https://charts.bitnami.com/bitnami

# install jenkins via helm
helm install --name banking-ant --set jenkinsUsername=admin,jenkinsPassword=admin123!,service.port=30080,service.httpsPort=30443 bitnami/jenkins

NOTES (SOME OF THESE ARE COPIED FROM THE OUTPUT):

** If you are running the export/echo commands below on windows for desktop, use something like git bash since they are unix commands **

#From Output below
-----

** Please be patient while the chart is being deployed **

1. Get the Jenkins URL by running:

** Please ensure an external IP is associated to the banking-ant-jenkins service before proceeding **
** Watch the status using: kubectl get svc --namespace default -w banking-ant-jenkins **

  export SERVICE_IP=$(kubectl get svc --namespace default banking-ant-jenkins --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}")
  echo "Jenkins URL: http://$SERVICE_IP/"

2. Login with the following credentials

  echo Username: user
  echo Password: $(kubectl get secret --namespace default banking-ant-jenkins -o jsonpath="{.data.jenkins-password}" | base64 --decode)
