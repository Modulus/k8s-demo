kubectl create ns demo
kubectl label namespace demo istio-injection=enabled
kubens demo

kubectl apply -f httpbin/httpbin.yml
kubectl apply -f httpbin/httpbin.istio.yml

$ export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
$ export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
$ export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')

curl -HHost:httpbin.example.com http://NGRESS_HOST:$INGRESS_PORT/status/200
curl -HHost:httpbin.example.com http://35.241.222.98/status/200

#GKE Stuff
gcloud compute firewall-rules create allow-gateway-http --allow tcp:$INGRESS_PORT
gcloud compute firewall-rules create allow-gateway-https --allow tcp:$SECURE_INGRESS_PORT

gcloud compute firewall-rules create allow-gateway-http --allow tcp:80
gcloud compute firewall-rules create allow-gateway-https --allow tcp:443

# Disable auto injection of sidecar container
kubectl label namespace default istio-injection-


