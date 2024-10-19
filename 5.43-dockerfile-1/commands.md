# assignment
* Dockerfiles are part process workflow and part art
* Take existing Node.js app and Dockerize it
* Make Dockerfile. Build it. Test it. Push it. (rm it). Run it.
* Expect this to be iterative. Rarely do I get it right the first time.
* Details in dockerfile-assignment-1/Dockerfile
* Use the Alpine version of the official 'node' 6.x image
* Expected result is web site at http://localhost 
* Tag and push to your Docker Hub account (free)
* Remove your image from local cache, run again from Hub

# commands
building image
* `docker image build -t mihransimonian/udemy-dockerfile-assignment .`

run image
* `docker container run -d --rm -p 80:3000 mihransimonian/udemy-dockerfile-assignment`

test image
* in browser -> `localhost:80`

stop container
* `docker stop container_name`

push to dockerhub
* `docker push mihransimonian/udemy-dockerfile-assignment`

list images
* `docker image ls`

delete image
* `docker image rm mihransimonian/udemy-dockerfile-assignment`

run image
* `docker container run -d --rm -p 80:3000 mihransimonian/udemy-dockerfile-assignment`