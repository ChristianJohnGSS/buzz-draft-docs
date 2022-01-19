# BUZZLINK

## Prerequisites

- Node.js
- Composer
- Docker

## Development Setup

### How to setup docker environment

1.  Go to `docker` directory

```
> cd  ./docker
```

2.  Start up docker containers

```
> docker-compose up -d
```

Note:

- At this time, schemaspy generates Database Scheme (found in `./docker/schemaspy/output`) from buzzlink database. If you want to regenerate schemaspy, excute `docker-compose up` again

- **Access ports**
  | PLATFORM | PORT |
  |------------|----------------|
  | php-apache | localhost:80 |
  | phpMyAdmin | localhost:8080 |

### **How to setup Laravel**

1.  Go to `src` directory:

    1. Copy `.env.example` to `.env` and update contents

    2. Run following commands:

    ```
    // For installing dependencies
    > npm install
    > composer install

    // For database migrations
    > php artisan migrate

    // For compiling assets and hot-reload for development
    > npm run watch
    ```

    Note:

    - On `.env` file, change `DB_HOST` from `mysql` to `127.0.0.1` for the following scenarios (then revert back to `mysql` if done and accessing website again):

      1. If an error displays about "**No such hosts is known**" after running `php artisan migrate` command.
      2. If doing any batch (src/app/Console/Commands) related tasks

    - If an error about view not found displays, run `composer dump-autoload` and refresh website

### **Enable queue database**

- You should use queue when you test sending email. The environment variable should be set in `.env` like below:

  ```
  QUEUE_CONNECTION=database
  ```

  Note:

  - "**database**" use queue in database and "**sync**" doesn't use queue

### **How to use minio**

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
