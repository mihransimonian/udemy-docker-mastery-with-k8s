prepare
* yml file is from compose-assignment-2 and i've changed the contents
* `cd` to folder `9.83-swarm-secrets-assignment-1`

start swarm
* `docker swarm init`

get password and see that it actually exists
* `echo "mypw" | docker secret create psql-pw -`
* `docker secret ls`

run
* `docker stack deploy -c docker-compose.yml drupal`

check functions
* goto browser `localhost:8080`
* install drupal, postgres, postgres, mypw, advanced, postgres, next

cleanup
* `docker swarm leave --force`
