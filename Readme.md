# Hateoas Spring Boot example behind a load balancer

This will create 3 VM running CentOS, two of them running the hateoas
spring boot example, and one using HAProxy two balance load between the network

To run this test is based on using Vagrant and Virtualbox

just run:

```
vagrant up
```

This will create 3 VM running CentOS, two of them running the hateoas
spring boot example, and one using HAProxy two balance load between the network

two access to the Load Balancer just point the browser to:

https://192.168.15.20/customers

the SSL certificate is autogenerated so trust any warning message on the browser

For accessing to the individual hateoas services point the browser to

http://192.168.15.21:9000/customers

or

http://192.168.15.22:9000/customers

To shutdown this environment:

```
vagrant destroy
```

---

Kubernetes Deployment

This can also be set inside a Kubernetes cluster (Tested on GCP) by running this commands
with a kubectl environment already defined:

```
cd k8s
kubectl -f k8s-rs.yml create
kubectl -f k8s-svc.yml create
kubectl -f k8s-haproxy.yml create
kubectl -f k8s-lb.yml create
```

check all the pods are up and running

```
kubectl get pods
```

To know which IP to connect, check for the External IP using

```
kubectl get svc haproxy-lb
```

https://EXTERNALIP/customers
