# ELK Demo
Quick demonstration of how to configure an ELK stack so it receives JSON logs from clients, using docker-compose.

## Getting Started
Make sure the necessary Docker tools are installed:

- Docker (e.g. Docker For Mac)
- docker-compose

## Running
Spin up the services in the compose file:

``` shell
docker-compose up -d
```

Tail the logs:

``` shell
docker-compose logs -f
```

>  It's handy to have a dedicated window/tab with logs so you can easily check for errors in the following steps (e.g. service fails to start, logstash fails to send data to elasticsearch, etc.)

Navigate to Kibana dashboard in your browser: http://localhost:5601 

> The port above comes from the port exposed to the bridge network in docker-compose.yml (5601:5601)

You should see this when you first open Kibana:

![dash](/docs/imgs/kibana-dash.png)

Click on the "Management" tab in the sidebar on the left, then "Index Patterns," where you should see a warning about "no data":

![no data](/docs/imgs/no-data.png)

You'll need to send data to build an index. To do this, either send client logs to logstash (localhost:5000, see the docker-compose ports section for `logstash`), or send it manually using curl:

``` shell
curl -X POST \
     -H 'Content-Type: application/json' \
     -d '{"message": {"hello": "world"}}' \
     localhost:5000
```

Click "Check For New Data" in Kibana, and you should see a option to specify a new logstash index pattern:

![index-pattern](/docs/imgs/index-pattern.png)

Specify `logstash-*`, click "Next," and click the `@timestamp` field from the drop-down. Once that's done, you should be able to click on the "Discover" tab in the left sidebar and see your log messages:

![hello world](/docs/imgs/hello-world.png)
