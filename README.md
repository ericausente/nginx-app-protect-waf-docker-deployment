# nginx-app-protect-waf-docker-deployment
NGINX App Protect WAF Docker Deployment Basic Testing

Reference Documentation:
```
https://clouddocs.f5.com/training/community/nginx/html/class5/module3/lab1/lab1.html
https://docs.nginx.com/nginx-app-protect-waf/admin-guide/install/#general-docker-deployment-instructions
```

Docker Commands (make sure there are no competing applications using the port 80 as it may complain that exposing port TCP 0.0.0.0:80 -> 0.0.0.0:0: listen tcp 0.0.0.0:8080: bind: address already in use.
% lsof -i :80
```
DOCKER_BUILDKIT=1 docker build --no-cache --secret id=nginx-crt,src=nginx-repo.crt --secret id=nginx-key,src=nginx-repo.key -t app-protect .
docker images app-protect
docker run --name my-app-protect -p 80:80 -d app-protect
docker ps
```

TESTING the app-protect functionality: 
 
```
ps aux | grep -E '/bd|nginx’

# curl localhost/apple
200 OK | Passed | Request was: GET /nap/apple
 
root@ubuntu:~# curl 'localhost/apple?1=<script>'
<html><head><title>Request Rejected</title></head><body>The requested URL was rejected. Please consult with your administrator.<br><br>Your support ID is: 9108677460373260626<br><br><a href='javascript:history.back();'>[Go Back]</a></body></html>
root@ubuntu:~# grep -a 9108677460373260626 /var/log/app_protect/security.log | tr ',' '\n'

```
