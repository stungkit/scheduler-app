[![Codacy Badge](https://api.codacy.com/project/badge/Grade/70223d33e20d4ed59ea4e310dc38260d)](https://www.codacy.com/app/maestro/scheduler-app?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=maestro-server/scheduler-app&amp;utm_campaign=Badge_Grade)
[![Build Status](https://travis-ci.org/maestro-server/scheduler-app.svg?branch=master)](https://travis-ci.org/maestro-server/scheduler-app)
[![Maintainability](https://api.codeclimate.com/v1/badges/3a073f54d89d948c0c08/maintainability)](https://codeclimate.com/github/maestro-server/scheduler-app/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/3a073f54d89d948c0c08/test_coverage)](https://codeclimate.com/github/maestro-server/scheduler-app/test_coverage)

# Maestro Server #

Maestro Server is an open source software platform for management and discovery servers, apps and system for Hybrid IT. Can manage small and large environments, be able to visualize the latest multi-cloud environment state.

### Demo ###
To test out the demo, [Demo Online](http://demo.maestroserver.io "Demo Online")

# Maestro Server - Scheduler #

Scheduler App service to manage and execute jobs

- Schedule jobs, interval or crontab
- Requests chain jobs
- Modules
    - Webhook: Call URL request
    - Connections: Call Crawler task

**Core API, organized by modules:**

* Celery Beat (Mongo Scheduler)
* Worker - Webhook
* Worker - Connections
* Worker - Chain
* Worker - Chain Exec
* Worker - Depleted Job
* Worker - Notify Event

## TechStack ##
* Python <3.4
* Celery
* RabbitMq
* MongoDB

## Service relations ##
* Maestro Data

## Setup #

#### Installation by docker ####

```bash
version: '2'

services:
    scheduler:
        image: maestroserver/scheduler-maestro
        environment:
        - "MAESTRO_DATA_URI=http://data:5000"
        - "CELERY_BROKER_URL=amqp://rabbitmq:5672"
        - "MAESTRO_MONGO_URI=localhost"
        - "MAESTRO_MONGO_DATABASE=maestro-client"

    scheduler:
        image: maestroserver/scheduler-maestro-celery
        environment:
        - "MAESTRO_DATA_URI=http://data:5000"
        - "CELERY_BROKER_URL=amqp://rabbitmq:5672"
        - "MAESTRO_MONGO_URI=localhost"
        - "MAESTRO_MONGO_DATABASE=maestro-client"
```

#### Dev Env ####
```bash
cd devtools/

docker-compose up -d
```

Configure rabbitmq service in .env file

```bash
CELERY_BROKER_URL="amqp://localhost:5672"
CELERYD_TASK_TIME_LIMIT=30
```

Install pip dependences
```bash
pip install -r requeriments.txt
```

Run beat
```bash
celery -A app.celery beat -S app.schedulers.MongoScheduler --loglevel=info

or 

npm run beat
```

Run workers
```bash
celery -A app.celery worker --loglevel=info

or 

npm run worker
```

### Env variables ###

| Env Variables                | Example                  | Description                        |
|------------------------------|--------------------------|------------------------------------|
| MAESTRO_DATA_URI             | http://localhost:5005    | Data Layer API URL                 |
| CELERY_BROKER_URL            | XXXX                     | Rabbitmq URL                       |
| MAESTRO_MONGO_URI            | localhost                | Mongo URI                          |
| MAESTRO_MONGO_DATABASE       | maestro-client           | Mongo Database name                |


### Contribute ###

Are you interested in developing Maestro Server, creating new features or extending them?

We created a set of documentation, explaining how to set up your development environment, coding styles, standards, learn about the architecture and more. Welcome to the team and contribute with us.

[See our developer guide](http://docs.maestroserver.io/en/latest/contrib.html)