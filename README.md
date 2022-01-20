# BUZZLINK

## 1. Prerequisites

- Make sure to have the following tools installed on your machine before continuing:

  - Node.js
  - Composer
  - Docker

## 2. Development Setup

- Follow the instructions below after cloning the repository.

### 1. How to setup docker environment

1.  On project directory, go to `docker` folder

    ```
    cd  ./docker
    ```

2.  Run command below to start up docker containers (_make sure docker is running before executing the command_)

    ```
    docker-compose up -d
    ```

    Note:

    - At this time, schemaspy generates Database Scheme (found in `./docker/schemaspy/output`) from buzzlink database. If you want to regenerate schemaspy, excute `docker-compose up` again

3.  After all app containers are created successfully. You can try and access the following platforms below:

    | PLATFORM   | PORT           |
    | ---------- | -------------- |
    | php-apache | localhost:80   |
    | phpMyAdmin | localhost:8080 |

### 2. How to setup Laravel

- If you have accessed `php-apache` using `localhost:80` and an error about "**No such file or directory**" is displayed, follow instructions below:

- On project directory, go to `src` folder:

  1. Copy `.env.example` to `.env` and update contents, or paste provided `.env` file

  2. Then run following commands:

     ```
     // For installing dependencies
     npm install
     composer install

     // For database migrations
     php artisan migrate

     // For compiling assets and hot-reload for development
     npm run watch
     ```

     Note:

     - On `.env` file, change `DB_HOST` from `mysql` to `127.0.0.1` for the following scenarios (then revert back to `mysql` if done and accessing website again):

       1. If an error displays about "**No such hosts is known**" after running `php artisan migrate` command.
       2. If doing any batch (`src/app/Console/Commands`) related tasks

     - If an error about view not found displays, run `composer dump-autoload` and refresh website

  3. After database migration, import SQL dump for required data (will be provided).
  4. Try and access website again. Happy coding!

### 3. How to use minio

- When `docker-compose up` command is ran, minio creates bucket "**public-bucket**" and "**private-bucket**". Set read:write permission to both buckets
- Update `.env` file with contents below:

  ```
  AWS_ACCESS_KEY_ID=minio
  AWS_SECRET_ACCESS_KEY=minio123
  AWS_DEFAULT_REGION=us-east-1
  AWS_PUBLIC_BUCKET=public-file
  AWS_PRIVATE_BUCKET=private-file
  AWS_URL_LOCAL=http://localhost:9090
  AWS_URL_DOCKER=http://minio:9000

  AWS_LOCAL_PUBLIC_URL="${AWS_URL_LOCAL}/${AWS_PUBLIC_BUCKET}"

  AWS_PUBLIC_URL="${AWS_URL_DOCKER}/${AWS_PUBLIC_BUCKET}"
  AWS_PRIVATE_URL="${AWS_URL_DOCKER}/${AWS_PRIVATE_BUCKET}"
  BUCKET_ENDPOINT=false
  USE_PATH_STYLE_ENDPOINT=true
  ```

- Then execute `php artisan config:cache` command
- You can access minio landing page using `localhost:9090` with:

  - ID: minio
  - KEY: minio123

### Enable queue database

- You should use queue when you test sending email. The environment variable should be set in `.env` like below:

  ```
  QUEUE_CONNECTION=database
  ```

  Note:

  - "**database**" use queue in database and "**sync**" doesn't use queue
