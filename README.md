# nginx-app-protect-waf-docker-deployment
NGINX App Protect WAF Docker Deployment Basic Testing

Reference Documentation:
```
https://clouddocs.f5.com/training/community/nginx/html/class5/module3/lab1/lab1.html
https://docs.nginx.com/nginx-app-protect-waf/admin-guide/install/#general-docker-deployment-instructions
```

Docker Commands (make sure there are no competing :
```
DOCKER_BUILDKIT=1 docker build --no-cache --secret id=nginx-crt,src=nginx-repo.crt --secret id=nginx-key,src=nginx-repo.key -t app-protect .
docker images app-protect
docker run --name my-app-protect -p 80:80 -d app-protect
docker ps
```
