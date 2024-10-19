start container
* `cd` to the folder with the yml
* `docker compose up`

check if runs using webbrowser
* goto URL `localhost:8080`
* Note during your login stuff on the web UI;
* select `postgres`
* db and username = `postgres`, password is from config
* advanced options, replace localhost with name of postgres service from yml, note this section is hidden
* leave number default port number

stop container
* `ctrl+c`

cleanup
* `docker compose down`