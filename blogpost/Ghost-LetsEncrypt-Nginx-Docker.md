I'm become increasingly fascinated with the concept of DevOps & what that means. I've noticed everyone has their own take as to what DevOps means, to me its creating solutions that encourage & enable fast-paced changes or releases. 

It's been a while since I have gone through the exercise of launching a blog. My first one I bootstrapped together and spent entirely too much time trying to figure out button spacing. This time I wanted to create something fast, light, & easy to throw posts onto. 

Building out AWS instance to host this blog with Ghost + Let's Encrypt + Nginx + Docker

# Prerequisites:
Built out an AWS AMI ec2 instance  
 * Great guide by [Tania Rascia](https://www.taniarascia.com/getting-started-with-aws-setting-up-a-virtual-server/).  

# Deploying via a container:
 * Build-out a docker-compose.yml config 
 * Installed Docker & Docker-Compose on linux instance 
 * Deploy config to server 
 * Run Docker commands 
 * Blog your socks off 


## Build-out a docker-compose.yml config
Using these images that handle the majority of the legwork.

image: `jwilder/nginx-proxy`
[Automated nginx proxy for Docker containers using dockergen](https://github.com/jwilder/nginx-proxy)

image: `jrcs/letsencrypt-nginx-proxy-companion`
[LetsEncrypt companion container for nginx-proxy](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion)

image: `ghost:latest`
[Ghost is a free and open source blogging platform written in Java Script](https://github.com/docker-library/ghost)

## Installed Docker & Docker-Compose on linux instance 
[Amazon's Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html)

Install Docker
 
```
sudo yum install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
```
Log out

Verify ec2-user no longer needs sudo

```
docker info 
```
## Deploy config to server 
Install Git
```
yum install git
git clone https://github.com/kcirtapfromspace/project-ghost-nginx-docker
```
## Run Docker commands
### Build up

check for any build errors. 
``` 
docker-compose up
```

Run in detached mode
```
docker-compose up -d
```

Login to container (useful in seeing how configs are deployed for nginx, if you have not mapped them to a local directory)

See your containers 
```
$ docker ps

CONTAINER ID        IMAGE                                    COMMAND   CREATED             STATUS              PORTS   NAMES
2714d1c0f301        jrcs/letsencrypt-nginx-proxy-companion   "/bin/bash /app/entr…"   30 minutes ago      Up 30 minutes   project-ghost-nginx-docker_nginx-proxy-companon_1
76cbe4caa211        ghost:alpine                             "docker-entrypoint.s…"   30 minutes ago      Up 30 minutes       0.0.0.0:8080->2368/tcp   project-ghost-nginx-docker_ghost_1
aa82282f185c        jwilder/nginx-proxy:alpine               "/app/docker-entrypo…"   30 minutes ago      Up 30 minutes       0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   project-ghost-nginx-docker_nginx-proxy_1
```

open bash for specific container ID(you'll see your shell change)
```
$ docker exec -it aa82282f185c bash
bash-4.4#
```
### Tear down 

```
docker-compose down
```

# Checking your work:

## https & headers  
Modern TLS Settings  
    https://www.ssllabs.com/ssltest/analyze.html?d=patrickdeutsch.us&latest   
Security Headers  
    https://securityheaders.com/?q=patrickdeutsch.us&followRedirects=on   

# still working out  
* need to remove tls1.0 
* need to enable tls1.3 
* need to adjust security headers 
* need to setup www.redirect  