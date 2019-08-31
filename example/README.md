# Docker Template for Rails

This repository serves as an educational docker template over which to build a rails app.

_Special thanks to [@daleal](https://github.com/daleal)._

## Prerequisites

- Install [Docker](https://docs.docker.com/install/).
- Install [docker-compose](https://docs.docker.com/compose/install/) (for linux).

## Getting Started

- Copy files & change app name from `example` on `Dockerfile`, `entrypoint.sh` and `docker-compose.yml` to your app's name.

- Create env file:

    ```bash
    touch .env
    ```

- Create a rails app:

    ```bash
    docker-compose run web rails new . --force --no-deps --database=postgresql
    ```

- Build the Docker image:

    ```bash
    docker-compose build
    ```

- Get ownership of the files (on linux they are owned by root when created):

    ```bash
    sudo chown -R $USER:$USER .
    ```

- Connect the database (replace the content of `config/database.yml` with the following, change `{example}` with your app's name):

    ```yaml
    default: &default
      adapter: postgresql
      encoding: unicode
      host: db
      username: postgres
      password:
      pool: 5

    development:
      <<: *default
      database: {example}_development


    test:
      <<: *default
      database: {example}_test
    ```

- Create the database:

    ```bash
    docker-compose run web rails db:create
    ```

- Run the app:

    ```bash
    docker-compose up
    ```

## Useful Commands

- Build the Docker image:

    ```bash
    docker-compose build
    ```

- Start the server:

    ```bash
    docker-compose up
    ```

- Stop the server:

    ```bash
    docker-compose down
    ```

- Run rails `command` (`command` = `rails ...`):

    ```bash
    docker-compose run web `command`
    ```

- Get ownership of files created by rails generate (on linux they are owned by root when created):

    ```bash
    sudo chown -R $USER:$USER .
    ```

## Common errors
- `Couldn't connect do Docker daemon`. Fix: run the command again with `sudo` at the beginning (Linux problem).
- Error when creating the database. Fix: run `docker-compose up`, `docker-compose down` and run the command again.

## Run an existing app
If your coworker already created the app and you want to run the existing up:
- Clone the repo.
- Build the containers (`docker-compose build`).
- Create the database at your computer (`docker-compose run web rails db:create`).
- Run the app (`docker compose up`).