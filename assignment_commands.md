# Description
This file contains the commands used in assingments, except for those that were bigger, those are referred to on Github itself and Dockerhub where applicable.


## Contents
[ToC]


# 4.22 assignment - manage multiple containers
## description
* docs.docker.com and `--help` are your friend
* Run a nginx, a mysql, and a httpd (apache) server
* Run all of them `--detach` (or `-d`), name them with `--name`
* nginx should listen on 80:80, httpd on 8080:80, mysql on 3306:3306
* When running mysql, use the `--env` option (or `-e`) to pass in `MYSQL_RANDOM_ROOT_PASSWORD=yes`
* Use docker container logs on mysql to find the random password it created on startup
* Clean it all up with `docker container stop` and `docker container rm` (both can accept multiple names or ID's)
* Use `docker container ls` to ensure everything is correct before and after cleanup

## commands
* `docker run --detach --publish 80:80 --name nginx nginx`
* `docker run --detach --publish 8080:80 --name httpd httpd`
* `docker run --detach --publish 79:3306 --name mysql --env MYSQL_RANDOM_ROOT_PASSWORD=yes mysql`
* `docker container logs mysql | grep -i password`

test nginx
* `curl localhost`

test
* `curl localhost:8080`
* `docker stop nginx httpd mysql`
* `docker rm nginx httpd mysql`
* `docker container ls -a`
* `docker container rm mysql httpd nginx`


# 4.31 assignment - cli-testing
## assignment
* Use different Linux distro containers to check curl cli tool version
* Use two different terminal windows to start bash in both centos:7 and ubuntu:14.04, using `-it`
* Learn the `docker container run —rm` option so you can save cleanup
* Ensure curl is installed and on latest version for that distro
* ubuntu: `apt-get update && apt-get install curl`
* centos: `yum update curl`
* Check `curl --version`

## commands
Terminal 1
* `docker run -it --rm --name ubuntu ubuntu:14.04`
* `apt-get update && apt-get install curl`
* `curl --version`
* 7.35.0
* `exit`

Terminal 2
* `docker run -it --rm --name centos centos:7`
* `yum update curl` -> one of the yum repo not available, so I skip this step
* `curl --version` (not updated)
* 7.29.0
* `exit`


# 4.34 assignment - dns round robin
## assignment
* Ever since Docker Engine 1.11, we can have multiple containers on a created network respond to the same DNS address
* Create a new virtual network (default bridge driver)
* Create two containers from 'elasticsearch:2' image
* Research and use `--network-alias search` when creating them to give them an additional DNS name to respond to
* Run `alpine nslookup search` with `--net` to see the two containers list for the same DNS name
* Run `centos curl -s search:9200` with `--net` multiple times until you see both "name" fields show

## commands
Create new network
* `docker network create my_elastic_network`

Make two docker containers elasticsearch connected to my network
* `docker run -d -it --name elasticsearch1 --network my_elastic_network --network-alias elastic elasticsearch:2`
* `docker network inspect my_elastic_network`
* `docker container inspect elasticsearch1`
* `docker run -d -it --name elasticsearch2 --network my_elastic_network --network-alias elastic elasticsearch:2`
* `docker container inspect elasticsearch2`

Run alpine nslookup
* `docker run --rm -it --network my_elastic_network alpine`
* `nslookup elastic` (the name of my DNS alias) -> note multiple IP
* `exit`
* A nicer way; `docker run --rm --net my_elastic_network alpine nslookup elastic`

Run centos
* `docker run --rm -it --network my_elastic_network centos`
* `curl -s elastic:9200` -> note different cluster_uuid
* `exit`
* A nicer way; `docker run --rm --net my_elastic_network centos curl -s elastic:9200`

Cleanup
* `docker container rm -f elasticsearch1 elasticsearch2`


# 5.43 assignment - dockerfile
## assignment
* Original assignment [here](https://github.com/BretFisher/udemy-docker-mastery/tree/main/dockerfile-assignment-1)

## commands
* My answers with Dockerfile and commands on Github [here](5.43-dockerfile-1/)
* My answer on Dockerhub [here](https://hub.docker.com/r/mihransimonian/udemy-dockerfile-assignment)


# 6.52 assignment - database upgrades
## assignment
* Database upgrade with containers
* Create a postgres container with named volume 'psql-data' using version 9.6.1
* Use Docker Hub to learn VOLUME path and versions needed to run it
* Check logs, stop container
* Create a new postgres container with same named volume using 9.6.2
* Check logs to validate (this only works with patch versions, most SQL DB's require manual commands to upgrade DB's to major/minor versions, i.e. it's a DB limitation not a container one)
* Note;
* Postgres requires a password to start
* Postgres needs a newer version to support arm64 and Apple Silicon

## commands
* `docker run -d --name psql-15-1 -v psql-data:/var/lib/postgresql/data -e POSTGRES_HOST_AUTH_METHOD=trust postgres:15.1`
* `docker logs psql-15-1`
* `docker volume ls`
* `docker container stop psql-15-1`
* `docker run -d --name psql-15-2 -v psql-data:/var/lib/postgresql/data -e POSTGRES_HOST_AUTH_METHOD=trust postgres:15.2`
* `docker logs psql-15-2`


# 6.56 assignment - bind mounts
* needs files from original repo [here](https://github.com/BretFisher/udemy-docker-mastery/tree/main/bindmount-sample-1)
* I've copied them to my Github [here](6.56-copy-bindmount-sample-1\)

## assignment
* Resource; https://jekyllrb.com/
* Use a Jekyll "Static Site Generator" to start a local web server
* Don't have to be web developer: this is example of bridging the gap between local file access and apps running in containers
* source code is in the course repo under bindmount-sample-1
* We edit files with editor on our host using native tools
* Container detects changes with host files and updates web server
* start container with
* `docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyllserve`
* Refresh our browser to see changes
* Change the file in `_posts\` and refresh browser to see changes


## commands
Start two terminals to ubuntu.

Terminal 1
* `cd` to the bindmount-sample-1 folder of the repository
* `docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve`
- this is the [website](https://hub.docker.com/r/bretfisher/jekyll-serve)

Terminal 2
* `cd` to the bindmount-sample-1 folder of the repository
* change the file in `_posts\` using nano

Browser
* goto -> `localhost:80`
* refresh page whilst you change the file in terminal 2
* note terminal 1 logs change when you change the file in terminal 2

Finish off in terminal 1
* ctrl + c
* `docker container prune`


# 7.63 assignment - compose 1
## assignment
* Original assignment [here](https://github.com/BretFisher/udemy-docker-mastery/tree/main/compose-assignment-1)

## commands
* my compose.yml and commands on Github [here](7.63-compose-1/)


# 7.67 assignment - compose 2
## assignment
* Original assignment [here](https://github.com/BretFisher/udemy-docker-mastery/tree/main/compose-assignment-2)

## commands
* my compose.yml and commands on Github [here](7.67-compose-2/)


# 8.73 assignment - create a 3-node Swam
## assignment
* Create a swarm with 3 nodes

## configuration
* I used https://labs.play-with-docker.com/ to make a multi-node setup

## commands
start three containers (three different IP) on the UI, take the IP of one of them and run
* `docker swarm init --advertise-addr <IP>`

join node as worker
* `docker swarm join --token <token> <IP:PORT>`

gather token for worker or manager
* `docker swarm join-token worker`
* `docker swarm join-token manager`

promote role of node
* `docker node ls`
* `docker node update --role manager node2`

demote role of node
* `docker node ls`
* `docker node update --role worker node2`

create a service
* `docker service create --replicas 3 alpine ping 8.8.8.8`

see all running
* `docker service ls`

see only local node
* `docker node ps`

see specific node
* `docker node ps node2`

see full list of all nodes
* `docker service ps <service_name>`


# 9.77 assignment - multinode webapp
* needs files from original repo [here](https://github.com/BretFisher/udemy-docker-mastery/tree/main/swarm-app-1)
* I've copied them to my Github [here](9.77-copy-swarm-app-1/)

## assignment
* Using Docker's Distributed Voting App
* use swarm-app-1 directory in our course repo for requirements
* 1 volume, 2 networks, and 5 services needed
* Create the commands needed, spin up services, and test app
* Everything is using Docker Hub images, so no data needed on Swarm
* Like many computer things, this is art and science combined

## commands
* `cd` to folder swarm-app-1
* `docker volume create db-data`
* `docker swarm init`
* `docker network create --driver overlay app-backend`
* `docker network create --driver overlay app-frontend`
* `docker service create --name vote --network app-frontend -p 80:80 --replicas 2 bretfisher/examplevotingapp_vote:stable-20240815-4b6de29`
* `docker service create --name redis --network app-frontend --replicas 1 redis:3.2`
* `docker service create --name worker --network app-frontend --network app-backend --replicas 1 bretfisher/examplevotingapp_worker:stable-20240815-4b6de29`
* `docker service create --name db --network app-backend --replicas 1 --mount type=volume,source=db-data,target=/var/lib/postgresql/data -e POSTGRES_HOST_AUTH_METHOD=trust postgres:9.4`
* `docker service create --name result --network app-backend --replicas 1 -p 5001:80 bretfisher/examplevotingapp_result:gha-10403515579`

browser
* goto IP or 'localhost:80' and see the website for voting
* goto IP or 'localhost:5001' and see the website for the result

cleanup
* `docker service rm db redis result vote worker`
* `docker volume rm db-data`


# 9.83 assignment - create a stack with secrets and deploy
## assignment
* Original assignment [here](https://github.com/BretFisher/udemy-docker-mastery/tree/main/swarm-secrets-assignment-1)

## commands
* my compose.yml and commands on Github [here](9.83-swarm-secrets-assignment-1/)


# 11.93 assignment - secure docker with TLS
## assignment
The default registry install is rather bare bones, and is open by default, meaning anyone can push and pull images. You'll likely want to at least add TLS to it so you can work with it easily via HTTPS, and then also add some basic authentication.

These aren't actually that hard to setup, but do require some commands.  You can learn the basics by creating a self-signed certificate for HTTPS, and then enabling htpasswd  auth, which you'll add users too with basic cli commands.

For this assignment you'll use Play With Docker, a great resource for web-based docker testing and also has a library of labs built by Docker Captains and others, and supported by Docker Inc. 

## online tooling
I used https://training.play-with-docker.com/linux-registry-part2/ to do this excercise and putted my commands there.

The commands displayed on the webpage would not fully work for me. In the commands below I  gave my solution, by not using the entrypoint of the image but mounting a local folder of the local machine to the docker container.

## commands
make an ssl certificate and fill in what is asked
* `mkdir -p certs`
* `openssl req -newkey rsa:4096 -nodes -sha256 -keyout certs/domain.key -x509 -days 365 -out certs/domain.crt`

copy 'domain.crt' to let docker trust the certificate
* `mkdir /etc/docker/certs.d`
* `mkdir /etc/docker/certs.d/127.0.0.1:5000`
* `cp $(pwd)/certs/domain.crt /etc/docker/certs.d/127.0.0.1:5000/ca.crt`

restart docker
* `pkill dockerd`
* `dockerd > /dev/null 2>&1 &`

run the registry
* `mkdir registry-data`
multi-line command;
* `docker run -d -p 5000:5000 --name registry \`\
  `--restart unless-stopped \`\
  `-v $(pwd)/registry-data:/var/lib/registry -v $(pwd)/certs:/certs \`\
  `-e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \`\
  `-e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \`\
  `registry`

pulling from and pushing to the registry
* `docker pull hello-world`
* `docker tag hello-world 127.0.0.1:5000/hello-world`
* `docker push 127.0.0.1:5000/hello-world`
* `docker pull 127.0.0.1:5000/hello-world`

add https authentication for user moby and password gordon
* `mkdir auth`

this one would not work for me, the entrypoint in the image does not seem to work
* `docker run --entrypoint htpasswd registry:latest -Bbn moby gordon > auth/htpasswd`

make a local auth and mount later as volume
* `htpasswd -Bbn moby gordon > auth/htpasswd`

if registry is running
* `docker kill registry`
* `docker rm registry`

run with https
* `docker run -d -p 5000:5000 --name registry \`\
    `--restart unless-stopped \`\
    `-v $(pwd)/registry-data:/var/lib/registry \`\
    `-v $(pwd)/certs:/certs \`\
    `-v $(pwd)/auth:/auth \`\
    `-e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \`\
    `-e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \`\
    `-e REGISTRY_AUTH=htpasswd \`\
    `-e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \`\
    `-e "REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd" \`\
    `registry`

test the connection
* `docker login 127.0.0.1:5000`
* `docker pull 127.0.0.1:5000/hello-world`
* `docker logout 127.0.0.1:5000`


# 22 section assignment - Github Actions
## assignment
This assignment covers multiple classes and is all about Github actions. In [this repo](https://github.com/BretFisher/docker-ci-automation) example YAML files are available to be used in the assignment, together with using the [Github docs](https://docs.github.com/en/actions).

### assignment: easy mode
My [bretfisher/httpenv app](https://github.com/BretFisher/httpenv) is a simple, single app that can be easy to fork and setup your own custom GitHub Actions. It's already got a few basic GHA's to start with, so you can enhance those existing YAML files or delete the .github directory and start new.
Assignment: Advanced Mode

### assignment: advanced mode
The [bretfisher/voting app repo](https://github.com/BretFisher/example-voting-app), which you've seen in this course, is a "mono repo" of three apps in subdirectories. You can fork it and start to add GitHub Actions for each app. I've already got some basics in there like building images, so you can start with them or delete the .github directory and start fresh!

## setup
* Using VSCode on Windows, or website where mentioned

## actions
* using the website, forked the github 'advanced repo' [bretfisher/voting app repo](https://github.com/BretFisher/example-voting-app) into [mihransimonian/udemy-docker-masterycourse-assignment-github-actions](https://github.com/mihransimonian/udemy-docker-masterycourse-assignment-github-actions)

* I iteratively improved the workflows in [branch dev-workflow](https://github.com/mihransimonian/udemy-docker-masterycourse-assignment-github-actions/tree/dev-workflow)
* The [end result](https://github.com/mihransimonian/udemy-docker-masterycourse-assignment-github-actions/actions/runs/11314833370)

## commands
pull to local env in terminal cd to it and work on new branch
* `git clone https://github.com/mihransimonian/udemy-docker-masterycourse-assignment-github-actions.git`
* `cd udemy-docker-masterycourse-assignment-github-actions`
* `git checkout -b dev-workflow`

rename orginal workflow dir to start clean
* `mv .\.github\workflows\ .\.github\workflows-original\`

walked through the [Github docs](https://docs.github.com/en/actions/writing-workflows/quickstart)
* `git add .`
* `git commit -m 'try gitdocs for github actions'`
* `git push -u origin HEAD`

add github secrets for dockerhub
* in dockerhub, generate access token
* in Github UI add repository secret for username and token
* specific variable names are in the workflow YAMLs

from here on a loop starts, I ran through the examples and saved them on my github workflow doc in numbered steps, where everytime I;
* made a file in VSCode
* `git add .`
* `git commit -m <message>`
* `git push`

### specific deviations or other notes
* `if: false`  # condition ensures job is never executed
* Step 8 test for worker skipped, it requires docker compose which is already done in step 9
* Step 9 made specific `docker-compose.test-image.yml` files in root of repository per image, test commands used are very simple, but different for the worker as it is a .NET image
* Step 10 is a paid plan, so I added environment variable `run_ghcr: false` and `if: env.run_ghcr == 'true'` statements to prevent using GCHR
* Step 99
- altered because I do not have paid plan for GCHR with `run_ghcr: false`
- Added steps, see in yml `# new step to share image between different github runners (jobs)`, source: https://docs.docker.com/build/ci/github-actions/share-image-jobs/

handy sources used
* https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/evaluate-expressions-in-workflows-and-actions#always
* https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables#default-environment-variables
