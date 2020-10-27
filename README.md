rabbitmq-docker-compose-headstart
# RabbitMQ Docker Compose - Headstart

Based on "RabbitMQ Custom Docker Image with Custom Configuration and Definitions" at https://www.youtube.com/watch?v=I8QHPfMhqAU&feature=youtu.be

Code at https://github.com/ekim197711/springboot-rabbitmq

## Install Docker Engine

See https://docs.docker.com/engine/install/centos/

## Verify Docker Engine Version

Run the following command after having installed Docker:

```
docker --version
```

## Install Docker Compose

See https://docs.docker.com/compose/install/

```

```

## Verify Docker Compose Version

```
docker-compose --version
```

## Create a working directory for RabbitMQ

```
mkdir /etc/rabbitmq
```

## Create a definitions.json file

From your project directory, create a file called 'definitions.json' with the following content:

```
{
  "users": [
    {
      "name": "guest",
      "password": "guest",
      "tags": "administrator"
    },
    {
      "name": "john",
      "password": "s3cr3t",
      "tags": "administrator"
    }    
  ],
  
  "vhosts": [
    {
      "name": "/"
    }
  ],
  
  "permissions": [
    {
      "user": "guest",
      "vhost": "/",
      "configure": ".*",
      "write": ".*",
      "read": ".*"
    },
    {
      "user": "john",
      "vhost": "/",
      "configure": ".*",
      "write": ".*",
      "read": ".*"
    }
  ],
  
  "parameters": [],
  
  "policies": [],
  
  "exchanges": [
    {
      "name": "foo.exchange",
      "vhost": "/",
      "type": "fanout",
      "durable": true,
      "auto_delete": false,
      "internal": false,
      "arguments": {}
    }
  ],
  
  "queues": [
    {
      "name": "foo.bar",
      "vhost": "/",
      "durable": true,
      "auto_delete": false,
      "arguments": {}
    },
    {
      "name": "foo.qux",
      "vhost": "/",
      "durable": true,
      "auto_delete": false,
      "arguments": {}
    },
    {
      "name": "foo.waldo",
      "vhost": "/",
      "durable": true,
      "auto_delete": false,
      "arguments": {}
    }    
  ],
  
  "bindings": [
    {
      "source": "foo.exchange",
      "vhost": "/",
      "destination": "foo.bar",
      "destination_type": "queue",
      "routing_key": "foo.bar.#",
      "arguments": {}
    },
    {
      "source": "foo.exchange",
      "vhost": "/",
      "destination": "foo.qux",
      "destination_type": "queue",
      "routing_key": "foo.qux.#",
      "arguments": {}
    },
    {
      "source": "foo.exchange",
      "vhost": "/",
      "destination": "foo.waldo",
      "destination_type": "queue",
      "routing_key": "foo.waldo.#",
      "arguments": {}
    }
  ]
  
}
```
definitions.json

## Create a rabbitmq.conf file

Reference: https://www.rabbitmq.com/configure.html#config-file

Reference: https://www.rabbitmq.com/management.html

From your project directory, create a file called 'rabbitmq.conf' with the following content:

```
loopback_users.guest = false
listeners.tcp.default = 5672
hipe_compile = false
management.listener.port = 15672
management.listener.ssl = false
management.load_definitions = /etc/rabbitmq/definitions.json

```
rabbitmq.conf

## Create a Dockerfile file

From your project directory, create a file called 'Dockerfile' with the following content:

```
FROM rabbitmq:3-management

COPY rabbitmq.conf /etc/rabbitmq/rabbitmq.conf
COPY definitions.json /etc/rabbitmq/definitions.json
```
Dockerfile

## Create a docker-compose.yml file

From your project directory, create a file called 'docker-compose.yml' with the following content:

```
version: '3.7'
services: 
  myrabbitmq:
    build: .
    environment:
      - RABBITMQ_CONFIG_FILE=/etc/rabbitmq/rabbitmq.conf
      - RABBITMQ_SERVER_ADDITIONAL_ERL_ARGS=-rabbit log [{console,[{level,debug}]}]
    ports:
      - 15672:15672
      - 5672:5672
```
docker-compose.yml


## Run docker compose

From your project directory, , start up your application by running the following command:

```
docker-compose up
```

See also https://docs.docker.com/compose/gettingstarted/

## Browse the Management Console

Open https://localhost:15672 to see the Management Console.

You can log in either with the default account:

- Username: guest
- Password: guest

Or with the custom created account:

- Username: john
- Password: s3cr3t

## Send and receive messages

... to do.
