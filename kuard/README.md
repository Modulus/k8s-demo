kubectl create ns demo
kubectl label namespace demo istio-injection=enabled
kubens demo

kubectl apply -f kuard/kuard.yml
kubectl apply -f kuard/kuard.istio.yml

$ export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
$ export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
$ export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')

curl -HHost:httpbin.example.com http://NGRESS_HOST:$INGRESS_PORT
curl -HHost:httpbin.example.com http://35.241.222.98


# Disable auto injection of sidecar container
kubectl label namespace default istio-injection-


