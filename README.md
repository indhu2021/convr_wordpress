## Description
This project can be used to deploy docker stack consisting of Wordpress, MySQL and nginx. 

## Tools installation

### Python3 Installation
- Update yum repo `sudo yum update`
- Install python3 `sudo yum install python3`
- Check version `python3 --version`

#### Install python3.8
- Install pre-requisite packages `yum install gcc openssl-devel bzip2-devel libffi-devel -y`
- Download latest python package ` curl -O https://www.python.org/ftp/python/3.8.16/Python-3.8.16.tgz` 
- Extract the archive `tar -xzf Python-3.8.1.tgz`
- Installation `./configure --enable-optimizations make altinstall`
- Update default python3 to Python 3.8 `sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1000`
- Check the python3 version `sudo update-alternatives --config python3`

### Git
- install `yum install git -y`
- verify the version `git --version`
- Set config values 
    `git config --global user.name "indhu"`
    `git config --global user.email "indhuammu@gmail.com"`
- Validate set config `git config --list`

### Docker
- install docker `sudo yum install docker -y`
- Check docker version `docker --version`
    `docker info`
- Add non-root user to docker group `sudo usermod -aG dockerroot centos`
- Enable docker service `sudo systemctl enable docker.service` 
- Start docker daemon `sudo service docker start` 
- Verify if docker commands can be run as non-root user `docker ps`
    
### Docker Compose
- Download latest release `sudo curl -L "https://github.com/docker/compose/releases/download/v2.14.2/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose`
- Add execute permissions `sudo chmod +x /usr/local/bin/docker-compose`
- Check the version `docker-compose --version`
    
### nginx configuration
- Add nginx config to listen on port 8080 (refer docker-compose.yml) in server block

```
server {
listen 8080;
listen [::]:8080;
...
}
```
- Add log paths to help during debugging
```
access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;
``` 
- Proxy pass to be pointed to container port for Wordpress
```
proxy_pass http://wordpress:80/;
proxy_set_header      X-Real-IP $remote_addr;
proxy_set_header      X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header      Host $host;
```

## Instructions for local setup
- Checkout files to local `git clone <repo path>`
- `cd convr_wordpress`
- Deploy the stack `docker-compose up -d`
- Check container status `docker ps`

## Troubleshooting
- Check docker logs `docker logs <container_id>` from local
- SSH into the nginx container and check nginx logs `tail -f /var/log/nginx/access.log` and `tail -f /var/log/nginx/error.log`

## Docker commands
 -Login to the docker container
`docker exec -it container-name /bin/bash`
 
 -Docker stop and remove
 `docker-compose stop`

- docker cleanup
 `docker-compose down`
