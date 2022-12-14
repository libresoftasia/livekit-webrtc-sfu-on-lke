# How to deploy LiveKit WebRTC SFU in Linode Kubernetes Engine

## Install the NGINX Ingress Controller

In this section you will use Helm to install the NGINX Ingress Controller on your Kubernetes Cluster. Installing the NGINX Ingress Controller will create Linode NodeBalancers that your cluster can make use of to load balance traffic to your example application.

{{< note >}}
If you want to learn more about the NGINX Ingress Controller, check out Linode guide [Deploying NGINX Ingress on Linode Kubernetes Engine].(/docs/guides/how-to-deploy-nginx-ingress-on-linode-kubernetes-engine/).
{{</ note >}}

1.  Add the following Helm ingress-nginx repository to your Helm repos.
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```
2.  Update your Helm repositories.
```
helm repo update
```
3.  Install the NGINX Ingress Controller. This installation will result in a Linode NodeBalancer being created.
```
helm install ingress-nginx ingress-nginx/ingress-nginx
```
You will see a similar output:

{{< output >}}
An example Ingress that makes use of the controller:
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: example
    namespace: foo
  spec:
    ingressClassName: nginx
    rules:
      - host: www.example.com
        http:
          paths:
            - pathType: Prefix
              backend:
                service:
                  name: exampleService
                  port:
                    number: 80
              path: /
    # This section is only required if TLS is to be enabled for the Ingress
    tls:
      - hosts:
        - www.example.com
        secretName: example-tls 
{{</ output >}}

The ingress-nginx controller has been installed.
It may take a few minutes for the LoadBalancer IP to be available.
You can watch the status by running 
```
kubectl --namespace default get services -o wide -w ingress-nginx-controller
```
You will get an output like below:

{{< output >}}
NAME                       TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                      AGE   SELECTOR
ingress-nginx-controller   LoadBalancer   10.128.248.194   172.104.37.25   80:31535/TCP,443:32161/TCP   21h   app.kubernetes.io/component=controller,app.kubernetes.io/instance=ingress-nginx,app.kubernetes.io/name=ingress-nginx
{{</ output >}}






