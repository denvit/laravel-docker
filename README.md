## Docker for Laravel
A pretty simplified Docker workflow that sets up a LEMP stack and Redis for local Laravel development.
## Usage

To get started, make sure you have [Docker installed](https://docs.docker.com/docker-for-mac/install/).

Next, navigate in your terminal to the directory you cloned this, and build-up containers for the web server by running `docker-compose up -d --build site`.

After that completes, add or git clone your project into `/src` directory.

By running the `docker-compose up -d` with `site` parameter instead of just using `up -d`, ensures that only containers from the site service are brought up at the start, instead of all of the containers that are defined in `docker-compose.yml`. The following are built for our web server, with their exposed ports detailed:

- **nginx** - `:8080`
- **mysql** - `:3306`
- **php** - `:9000`

Other services have next ports:
- **redis** - `:6379`

I've included three additional containers for easier executing of composer, NPM, and artisan commands **without** having to have these tools installed on your local computer - each of these runs isolated in docker container. 

Examples of how you can use these commands from your project root directory:

- `docker-compose run --rm composer update`
- `docker-compose run --rm npm run dev`
- `docker-compose run --rm artisan migrate`

If you want to run commands from php container (like chmod or similar) use this:
```
docker exec -it php sh
```

## Persistent MySQL Storage

By default, whenever you bring down the Docker network using `docker-compose down`, your MySQL data will be removed after the containers are destroyed. If you would like to have persistent data that remains after bringing containers down and back up, do the following:

If you're using MySQL service:
1. Create a `mysql` folder in the project root, alongside the `nginx` and `src` folders.
2. Under the mysql service in your `docker-compose.yml` file, add the following lines:

```
volumes:
  - ./mysql:/var/lib/mysql:rw
```

And this if you're using MariaDB service:
1. Create a `mariadb` folder in the project root, alongside the `nginx` and `src` folders.
2. Under the mariadb service in your `docker-compose.yml` file, add the following lines:

```
volumes:
  - ./mariadb:/var/lib/mysql:rw
```

## Persistent Redis storage
Whenever you bring down the Docker network using `docker-compose down` will also remove the redis data, so if you would like to have persistent data do the following to the Redis service:

1. Create a `redis` folder in the project root, alongside the `nginx`, `mysql/mariadb` and `src` folders.
2. Under the redis service in your `docker-compose.yml` file, add the following lines:

```
volumes:
  - ./redis/data:/data:rw
```

## Additional services

If you want to add any additional service to your environment, go check on Official [Docker Hub](https://hub.docker.com/)