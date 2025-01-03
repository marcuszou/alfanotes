# AlfaNotes

by Marcus Zou

## Intro

This is a technotes WebApp utilizing the best open-sourced blogging platform of ghost.org;

## Run up AlfaNotes WebApp

Some pre-jobs:

```shell
mkdir alfanotes && cd alfanotes
## make 2 folders for 2 volumes engaged later
mkdir db ghost
## Create a docker-compose.yaml file
nano docker-compose.yaml
```

The contents of the docker-compose.yaml for a Production environment shall be like below:

```textfile
services:
  ghost:
    image: ghost:5-alpine
    restart: always
    ports:
      - 3001:2368
    environment:
      # see https://ghost.org/docs/config/#configuration-options
      database__client: mysql
      database__connection__host: db
      database__connection__user: root
      database__connection__password: Kanada2024!pass
      database__connection__database: alfanotes
      # this url value is just an example, and is likely wrong for your environment!
      url: http://localhost:3001
      # contrary to the default mentioned in the linked documentation, this image defaults to NODE_ENV=production
      # (so development mode needs to be explicitly specified if desired)
      # NODE_ENV: development
    volumes:
      - ghost:/var/lib/ghost/content

  db:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: Kanada2024!pass
    volumes:
      - db-prod:/var/lib/mysql

volumes:
  ghost:
  db-prod:
```

then running up the docker:

```shell
docker-compose up -d
## sudo docker-compose up -d
```

finally open a browser,
and type: http://localhost:3001 for the website;
or type: http://localhost:3001/ghost for the admin page.

## Pullover AlfaNotes WebApp

As simple as below:

```shell
# Take down the 2 containers
docker ps  ## Note down the container ID
docker stop d5e649827cff 863668a38b16
docker rm d5e649827cff 863668a38b16
docker ps  ## there is no such 2 containers any more

# Remove the images
docker images  ## list out the images and note down the 2 Image IDs
docker image rm 2d3a7de762e6 d58ac93387f6
docker images  ## there is no such 2 images any more
```

## Options for Development Env

Optionally run the WebApp using a Development Environment associated with `sqlite3` database by default as steps below:

```shell
cp docker-compose-dev.yaml docker-compose.yaml
docker-compose up -d
```

## Outro

You may go to https://ghost.org/ for more details.

## License

MIT
