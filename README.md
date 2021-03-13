# SiteSpectK8s

helm repo add bitnami [https://charts.bitnami.com/bitnami](https://charts.bitnami.com/bitnami) ← This is a Wordpress Chart that I am familiar with. [https://bitnami.com/stack/wordpress/helm](https://bitnami.com/stack/wordpress/helm)

helm install my-release bitnami/wordpress

kubectl get po ← to see the running instance of wordpress ( and any others running )

kubectl get scv ≤ to see the DNS for non cluster access once dns resolves

( kubectl get svc --namespace default -w my-release-wordpress ) to watch the deployment of the install. There is a mariadb pod as well as a my-release-wordpress pod that has the front end of the wordpress install exposed.

( this is about 20 mins in, waiting for the pod to be exposed )

minikube service my-release-wordpress ← this alllowed me to access the loadbalancer service at 127.0.0.1 on port 56383

For prometheus

INstalled prometheus operator

helm repo add prometheus-community [https://prometheus-community.github.io/helm-charts](https://prometheus-community.github.io/helm-charts)

helm repo add stable [https://kubernetes-charts.storage.googleapis.com/](https://kubernetes-charts.storage.googleapis.com/) ← this was deprecated so I used [https://charts.helm.sh/stable](https://charts.helm.sh/stable)

helm repo update

helm install prometheus prometheus-community/kube-prometheus-stack ← install and call it prometheus

kubectl get pods ← waiting for the pods to spin up

This spins up 2 stateful sets. " oper-prometheus " is the prometheus server. , " oper-aletermanager " is the alert manager

grafana was deployed : deployment.apps/promethus-grafana

kube-state-metrics : monitors the health of deployments,pods and statefulsets in the cluster and makes it available for prometheus to access

NodeExporter Daemon translates workernode metrics ( cpu usage, server load, etc ) to prometheus metrics

kubectl get configmaps ← displays the configmaps

kubectl get secrets ← shows the secrets used by the various services

prometheus-prometheus-kube-prometheus-prometheus > prom.yaml ← view the config and save in a file caleld prom.yaml

kubectl describe statefulset alertmanager-prometheus-kube-prometheus-alertmanager > alert.yaml ← the same for alert manager

kubectl describe deployment my-release-wordpress > wp.yaml ← for the wordpress deployment

kubectl describe deployment prometheus-kube-prometheus-operator > oper.yaml ← for the prometheus operator

kubectl get deployment > deploy.yaml ← get config for the entire deployment

kubectl logs prometheus-grafana-856b499fb9-lqsmd -c grafana ← to check the port that grafana is running on. listening on port 3000

kubectl port-forward deployment/prometheus-grafana 3000 ← exposes grafana on port 3000 ( creds admin, prom-operator ( default pw for the chart )

manage > dashboard > k8s>compute resources>pod/my-releae-mariadb-0 shows the metrics for the mariada pod for wordpress

manage>dashboard>k8s>compute resources>workload/my-release-wordpress/deployment shows metrics for the wordpress deployment

manage>dashboard>k8s>computeresources>cluster for metrics for the cluster at large

kubectl port-forward prometheus-prometheus-kube-prometheus-prometheus-0 9090 , the config for prometheus ( prom.yaml ) shows me that prometheus is listening on port 9090.

This took about 1.5hrs with writing notes.

Formatting the ReadMe to be clean vs raw notes took about 30 minutes
