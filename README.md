`docker build -t andylke/demo-k8s-static-website-nginx .`

```
[+] Building 5.2s (9/9) FINISHED
 => [internal] load build definition from Dockerfile                                                                       0.1s
 => => transferring dockerfile: 136B                                                                                       0.0s
 => [internal] load .dockerignore                                                                                          0.0s
 => => transferring context: 2B                                                                                            0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                                            4.7s
 => [auth] library/nginx:pull token for registry-1.docker.io                                                               0.0s
 => [1/3] FROM docker.io/library/nginx:alpine@sha256:da9c94bec1da829ebd52431a84502ec471c8e548ffb2cedbf36260fd9bd1d4d3      0.0s
 => [internal] load build context                                                                                          0.0s
 => => transferring context: 194B                                                                                          0.0s
 => CACHED [2/3] COPY default.conf /etc/nginx/conf.d/                                                                      0.0s
 => [3/3] COPY index.html /usr/share/nginx/html/                                                                           0.1s
 => exporting to image                                                                                                     0.1s
 => => exporting layers                                                                                                    0.1s
 => => writing image sha256:552b1d7baa8ffda52f040b90c439bea2839278c6c34d89756611553504ed7460                               0.0s
 => => naming to docker.io/andylke/demo-k8s-static-website-nginx
```

`docker images andylke/demo-k8s-static-website-nginx`

```
REPOSITORY                              TAG       IMAGE ID       CREATED         SIZE
andylke/demo-k8s-static-website-nginx   latest    552b1d7baa8f   2 minutes ago   23.4MB
```

`docker login`

```
Authenticating with existing credentials...
Login Succeeded
```

`docker push andylke/demo-k8s-static-website-nginx`

```
Using default tag: latest
The push refers to repository [docker.io/andylke/demo-k8s-static-website-nginx]
eb9e2214d79a: Pushed
acd4f9c7f44b: Pushed
6fda88393b8b: Pushed
a770f8eba3cb: Pushed
318191938fd7: Pushed
89f4d03665ce: Pushed
67bae81de3dc: Mounted from library/nginx
8d3ac3489996: Mounted from library/nginx
latest: digest: sha256:ad6afb6f0bf9f90f503beee9b006a099248178d6c6f76b7851db99f8a71a0b78 size: 1982
```

`kubectl create -f deployment.yaml`

```
deployment.apps/demo-k8s-static-website-nginx created
```

`kubectl get pods`

```
NAME                                            READY   STATUS    RESTARTS   AGE
demo-k8s-static-website-nginx-656fbd9fb-thgkw   1/1     Running   0          32s
```

`kubectl create -f service.yaml`

```
service/demo-k8s-static-website-nginx created
```

`kubectl get services`

```
NAME                            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
demo-k8s-static-website-nginx   NodePort    10.107.153.115   <none>        80:30954/TCP   24s
kubernetes                      ClusterIP   10.96.0.1        <none>        443/TCP        6d2h
```

`kubectl proxy`

```
Starting to serve on 127.0.0.1:8001
```

`curl http://localhost:30954/`

```
StatusCode        : 200
StatusDescription : OK
Content           : <html>
                    <body>
                            <h1>Welcome to Demo for Hosting a Static Website using Nginx on Kubernetes</h1>
                    </body>
                    </html>
RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    Accept-Ranges: bytes
                    Content-Length: 121
                    Content-Type: text/html
                    Date: Thu, 10 Mar 2022 07:54:57 GMT
                    ETag: "6229abf4-79"
                    Last-Modified: Thu, 10 Mar 2022 0...
Forms             : {}
Headers           : {[Connection, keep-alive], [Accept-Ranges, bytes], [Content-Length, 121], [Content-Type, text/html]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : System.__ComObject
RawContentLength  : 121
```

`CTRL + C`

`kubectl delete -f service.yaml`

```
service "demo-k8s-static-website-nginx" deleted
```

`kubectl delete -f deployment.yaml`

```
deployment.apps "demo-k8s-static-website-nginx" deleted
```

`kubectl get all`

```
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   6d2h
```
