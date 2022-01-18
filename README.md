# BUZZLINK

## Prerequisites

- Node.js
- Composer
- Docker

## Setup

### Development

- How to setup docker environment

  1. Go to docker directory

  ```
  cd  ./docker
  ```

  2. Start up docker containers

  ```
  docker-compose up -d
  ```

  Note:

  - At this time, schemaspy generates Database Scheme from buzzlink database. If you want to regenerate schemaspy, excute `docker-compose up` again.

- Access ports
  | PLATFORM | PORT |
  |------------|----------------|
  | php-apache | localhost:80 |
  | phpMyAdmin | localhost:8080 |
