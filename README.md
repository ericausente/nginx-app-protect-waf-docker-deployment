# nginx-app-protect-waf-docker-deployment
## NGINX App Protect WAF Docker Deployment Basic Testing


Run the following command to clone the repository. 
This will create a new directory called nginx-app-protect-waf-docker-deployment containing the contents of the repository.
Then Change to the repository directory. 

```
git clone https://github.com/ericausente/nginx-app-protect-waf-docker-deployment.git
cd nginx-app-protect-waf-docker-deployment
```

### Docker Commands 
(make sure there are no competing applications using the port 80 as it may complain that exposing port TCP 0.0.0.0:80 -> 0.0.0.0:0: listen tcp 0.0.0.0:80: bind: address already in use.
```
% lsof -i :80
```

Before proceeding with the below steps, make sure to populate the nginx-repo.crt and nginx-repo.key files with your own SSL certificates. Once done, run the following commands to build the Docker image and run the container:

```
DOCKER_BUILDKIT=1 docker build --no-cache --secret id=nginx-crt,src=nginx-repo.crt --secret id=nginx-key,src=nginx-repo.key -t app-protect .
docker images app-protect
docker run --name my-app-protect -p 80:80 -d app-protect
docker ps
```

### TESTING the app-protect functionality: 
 
```
% curl http://localhost
200 OK | Passed | Request was: GET /nap/ 

% curl 'http://localhost/?1<script>'     
<html><head><title>Request Rejected</title></head><body>The requested URL was rejected. Please consult with your administrator.<br><br>Your support ID is: 9608988821215377121<br><br><a href='javascript:history.back();'>[Go Back]</a></body></html>%

```

### Clean up 
```
docker stop my-app-protect 
docker rm my-app-protect 
docker image rm app-protect
```


### Reference Documentation:
```
https://clouddocs.f5.com/training/community/nginx/html/class5/module3/lab1/lab1.html
https://docs.nginx.com/nginx-app-protect-waf/admin-guide/install/#general-docker-deployment-instructions
```

# Troubleshooting Notes if the container is not running.
Here are a few things you can try to troubleshoot the issue:

```
    Check if the container was created: You can use the command docker ps -a to list all containers (including stopped ones). If the container was created, it should be listed here.

    Check the logs: You can use the command docker logs <container_id> to view the logs of the container. This can help you identify any errors that occurred during the container startup.

    Check the image: Make sure that the app-protect image exists and is valid. You can use the command docker images to list all the images on your system.

    Check if port 80 is available: The -p flag maps port 80 on the host to port 80 in the container. Make sure that port 80 is available on your system and not being used by another process. You can check this by running the command sudo lsof -i :80.

    Try running the container in foreground: Instead of running the container in detached mode (-d flag), try running it in the foreground using the command docker run --name my-app-protect -p 80:80 app-protect. This will show the output of the container in your terminal and help you identify any issues.
```

Sample logging server setup: 
```
~$ docker run -d -p 514:514 -p 514:514/udp -p 1601:1601 rsyslog/syslog_appliance_alpine

~$ docker ps
CONTAINER ID        IMAGE                             COMMAND                  CREATED             STATUS              PORTS                                                                NAMES
b7e6cdd8f84b        rsyslog/syslog_appliance_alpine   "/home/appliance/sta???"   8 minutes ago       Up 8 minutes        0.0.0.0:514->514/tcp, 0.0.0.0:1601->1601/tcp, 0.0.0.0:514->514/udp   heuristic_khorana

~$  docker exec -it b7e6cdd8f84b sh

# cd /logs/hosts/

```
