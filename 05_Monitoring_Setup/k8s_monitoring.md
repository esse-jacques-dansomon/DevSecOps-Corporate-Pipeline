# Kubernetes Monitoring with Pormetheus and Grafane

Steps to Install Prometheus:
--------------------------------

```shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext
minikube service prometheus-server-ext
helm uninstall prometheus
```


Steps to install Grafana:
--------------------------

```shell
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install grafana stable/grafana
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext
minikube service grafana-ext
```

### To get user name and password in Grafana:

```shell
kubectl get secret --namespace default grafana -o yaml
echo "RkpwY21aTFNXRDVJN3Z4RWFFUjlibkV1SDBDbnFBendadmc0bmROZQ==" | openssl base64 -d ; echo
````

or
```shell
kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```


```
Dashboards: https://grafana.com/grafana/dashboards/6417
Grafana playlist: https://www.youtube.com/playlist?list=PLVx1qovxj-akOGKSoQ5sGc5ZRfH8Tpnow
Prometheus Playlist: https://www.youtube.com/playlist?list=PLVx1qovxj-anCTn6um3BDsoHnIr0O2tz3
```