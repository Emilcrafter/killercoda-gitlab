# Step 1 - Run GitLab Docker container

## First, define the GITLAB_HOME environment variable

```bash
export GITLAB_HOME=/srv/gitlab
```

## Next, run the container

```bash

sudo docker run --detach  \
  --hostname gitlab.example.com \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  --shm-size 256m \
  gitlab/gitlab-ee:latest
```

## Explanation:
  * We run the container in `--detach`ed mode, meaning it will run in the background. 
  * We give it a `--hostname` or it will use the container ID as the hostname. 
  * We publish ports `443`, `80` and `22` to the host. 
  * We name the container `gitlab` and set it to `--restart always` to make sure that the container is always running unless we explicitly stop it.
  * We mount the `$GITLAB_HOME` directory to the container's `/etc/gitlab`, `/var/log/gitlab` and `/var/opt/gitlab` directories. 
  * We set the shared memory size to `256m` to avoid any issues with GitLab's memory consumption.

## Verify installation
* To verify that the GitLab container is running, use the `docker ps` command:
    
```bash
sudo docker ps
```

* You should see output similar to the following:
    
```
CONTAINER ID        IMAGE                        COMMAND                  CREATED             STATUS              PORTS                                                                NAMES

9e6c8e6d4bba        gitlab/gitlab-ee:latest      "/assets/wrapper"        2 minutes ago       Up 2 minutes
```