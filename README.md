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

Troubleshooting Notes if the container is not running.

```
Here are a few things you can try to troubleshoot the issue:

    Check if the container was created: You can use the command docker ps -a to list all containers (including stopped ones). If the container was created, it should be listed here.

    Check the logs: You can use the command docker logs <container_id> to view the logs of the container. This can help you identify any errors that occurred during the container startup.

    Check the image: Make sure that the app-protect image exists and is valid. You can use the command docker images to list all the images on your system.

    Check if port 80 is available: The -p flag maps port 80 on the host to port 80 in the container. Make sure that port 80 is available on your system and not being used by another process. You can check this by running the command sudo lsof -i :80.

    Try running the container in foreground: Instead of running the container in detached mode (-d flag), try running it in the foreground using the command docker run --name my-app-protect -p 80:80 app-protect. This will show the output of the container in your terminal and help you identify any issues.
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
