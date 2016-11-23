# Gambit Services

Gambit is powered by several micro-services working in unison. This repo contains git submodules & a docker setup for each making local development much faster.

**note**: This repo, nor Docker, is used in production at this time. The setup does heavily mimic the production environment.

## Setup

> :memo: When you install [Docker for Mac ](https://docs.docker.com/docker-for-mac/) it comes bundled with Docker Compose.

1. `git clone https://github.com/dosomething/gambit-services`
2. `cd gambit-services`
3. `git submodule update --init`
4. `docker-compose up`

## Creating new micro services

Creating new micro services for Gambit is fairly straight forward.

1. First, use our custom [Yeoman Scaffolding](https://github.com/DoSomething/generator-gambit) to generate a project.
2. Publish your service in a new Github repo.
3. Link the service in this repository as a sub module,
`git submodule add -b master git@github.com:dosomething/gambit-example-service.git`
4. Add it to the `docker-compose.yml` so it can launch in this container with the other services. Just copy the following template & replace the name where designated.

```yml
${service-name}:
  build: ./${service-name}
  links:
    - mongo
    - rabbit
  environment:
    - MONGODB_URI=mongodb://mongo_1:27017/${service-name}
    - RABBIT_URI=amqp://rabbitmq_1:5672/dosomething
  volumes:
    - ./${service-name}/:/app
```

**note:** Some groups require links between services, in that case simply define a link between the two names & set the environment variables. See gambit & messaging-groups as an example.
