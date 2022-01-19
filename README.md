# BUZZLINK

## Prerequisites

- Node.js
- Composer
- Docker

## Setup

### Development

1. **How to setup docker environment**

   1. Go to `docker` directory

   ```
   > cd  ./docker
   ```

   2. Start up docker containers

   ```
   > docker-compose up -d
   ```

   Note:

   - At this time, schemaspy generates Database Scheme from buzzlink database. If you want to regenerate schemaspy, excute `docker-compose up` again

   - **Access ports**
     | PLATFORM | PORT |
     |------------|----------------|
     | php-apache | localhost:80 |
     | phpMyAdmin | localhost:8080 |

2. **How to setup Laravel**

   1. Go to `src` directory:

      1. Copy `.env.example` to `.env` and update contents

      2. Run following commands:

      ```
      // for installing dependencies
      > npm install
      > composer install

      // for database migrations
      > php artisan migrate

      // for compiling assets and hot-reload for development
      > npm run watch
      ```

      Note:

      - On `.env` file, change `DB_HOST` from `mysql` to `127.0.0.1` for the following scenarios (then revert back to `mysql` if done and accessing website again):
        1. If an error displays after running `php artisan migrate` about "**No such hosts is known**".
        2. If doing batch (src/app/Console/Commands) related tasks
