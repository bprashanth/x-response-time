# Nginx response time

Deploy the backend and frontend

```console
$ kubectl create -f ./
service "backend" created
replicationcontroller "backend" created
service "nginx-frontend" created
replicationcontroller "nginx-frontend" created
```

Wait for the frontend to get a loadbalancer

```console
$ kubectl get svc nginx-frontend -w
NAME             CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
nginx-frontend   10.0.247.169   <pending>     80:31673/TCP   20s
nginx-frontend   10.0.247.169   35.184.73.62   80:31673/TCP   1m
```

Look for the response time, this is directly from the backend which does nothing
by sleep and echo the time spent sleeping

```console
$ curl 35.184.73.62/sleep
0.50099992752075
```

Look for the header added by the proxy

```console
$ curl -X HEAD -I 35.184.73.62/sleep
HTTP/1.1 200 OK
Server: nginx/1.11.1
Date: Sat, 11 Feb 2017 02:23:55 GMT
Content-Type: text/plain
Connection: keep-alive
X-Response-Time: 0.503
X-Upstream-Response-Time: 1486779835.317
```

