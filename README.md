# Sensu Client inside a Docker Container
This project allows you to setup Sensu in a docker container.

It's running phusion/baseimage which is currently based on Ubuntu 14.04 LTS

To get Sensu up and running correctly there are a few mappings you have to provide. The easiest way to do this is to use docker-compose with the sample `docker-compose.yml`

If you want to manage the mappings yourself these are the configs you'll need to map into the container:
* `/etc/sensu/ssl/cert.pem`
* `/etc/sensu/ssl/key.pem`
* `/etc/sensu/conf.d/rabbitmq.json`
* `/etc/sensu/conf.d/client.json`

To setup additional plugins and monitoring, you'll need to to also map `/etc/sensu/plugins`
