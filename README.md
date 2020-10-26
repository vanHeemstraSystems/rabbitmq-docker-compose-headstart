rabbitmq-docker-compose-headstart
# RabbitMQ Docker Compose - Headstart

Based on "RabbitMQ Custom Docker Image with Custom Configuration and Definitions" at https://www.youtube.com/watch?v=I8QHPfMhqAU&feature=youtu.be

Code at https://github.com/ekim197711/springboot-rabbitmq

## Create a definitions.json file

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
  
  "permission": [
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
      
    }
  ]
  
}
```
definitions.json

## Create a rabbitmq.conf file

Reference: https://www.rabbitmq.com/configure.html#config-file

Reference: https://www.rabbitmq.com/management.html

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

```
FROM rabbitmq:3-management

COPY rabbitmq.conf /etc/rabbitmq/rabbitmq.conf
COPY definitions.json /etc/rabbitmq/definitions.json
```
Dockerfile

## Create a docker-compose.yml file

```

```
docker-compose.yml
