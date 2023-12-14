# testproject1
EC2 Deployment of  RreactJS/NodeJS and Django APP inside docker container by using nginx reverse proxy

At first create EC2 instance with inbound rules for ssh and http ports. Write a bash script in User Data 

!#/bin/bash
sudo apt-get update
sudo apt-get install Nginx -y
sudo apt-get install docker.io -y

This will update the apt package, install Nginx and docker while creating EC2 instance.  


git clone    https://github.com/saurabhsinghaniyas/reactdjango
Building Docker file
FROM 	python:3.9
WORKDIR /app/backend
COPY 	requirements.txt /app/backend
RUN  pip install -r requirements.txt
COPY 	.  /app/backend
EXPOSE 8000
CMD python /app/backend/manage.py runserver 0.0.0.0:8000


Now, Build the docker image and run in detach mode :               
docker  build  -t  notes-app   .
docker run -dit -p 8000:8000 --name my-notes-container notes-app:latest
 

Nginx config for Reverse Proxy Server
vim 	etc/nginx/sites-enabled/ defaults	 [reverse proxy server]
location	 / {
             proxy_pass http://127.0.0.1:8000;
             # First attempt to serve request as file, then
             # as directory, then fall back to displaying a 404.
             #try_files $uri $uri/ =404;
}
 location	 /api {
             proxy_pass http://127.0.0.1:8000/api;
             # First attempt to serve request as file, then
             # as directory, then fall back to displaying a 404.
             #try_files $uri $uri/ =404;
}

