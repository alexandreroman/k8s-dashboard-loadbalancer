# Expose your Kubernetes Dashboard using a `LoadBalancer`

This repository shows how to simply expose your Kubernetes Dashboard using
a `LoadBalancer` resource. Using this configuration, you don't need to use
`kubectl proxy` to get access to the dashboard: just use the allocated IP
address in your browser.

## How to use it?

Create a `LoadBalancer` resource using this command:
```bash
$ kubectl apply -f k8s-dashboard-loadbalancer.yml
service/kubernetes-dashboard-lb created
```

This file defines a `LoadBalancer` exposing port `8443` from the Kubernetes
dashboard with an allocated IP address on port `443`:
```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: kubernetes-dashboard-lb
  namespace: kube-system
spec:
  type: LoadBalancer
  ports:
    - port: 443
      protocol: TCP
      targetPort: 8443
  selector:
    k8s-app: kubernetes-dashboard
```

Wait a few seconds until an IP address is ready to serve traffic on port `443`:
```bash
$ kubectl -n kube-system get svc
NAME                            TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)         AGE
heapster                        ClusterIP      10.100.200.91    <none>        8443/TCP        30d
kube-dns                        ClusterIP      10.100.200.2     <none>        53/UDP,53/TCP   30d
kubernetes-dashboard            NodePort       10.100.200.149   <none>        443:32283/TCP   30d
kubernetes-dashboard-lb         LoadBalancer   10.100.200.138   1.2.3.4       443:30006/TCP   7m38s
metrics-server                  ClusterIP      10.100.200.205   <none>        443/TCP         30d
monitoring-influxdb             ClusterIP      10.100.200.253   <none>        8086/TCP        30d
```

You're done! Look at entry `kubernetes-dashboard-lb` to find the IP address to use.

## Contribute

Contributions are always welcome!

Feel free to open issues & send PR.

## License

Copyright &copy; 2019 [Pivotal Software, Inc](https://pivotal.io).

This project is licensed under the [Apache Software License version 2.0](https://www.apache.org/licenses/LICENSE-2.0).
