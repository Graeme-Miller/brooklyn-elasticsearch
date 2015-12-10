# brooklyn-elasticsearch
Pure Brooklyn YAML blueprint to deploy a cluster of Elasticsearch nodes.

Currently you can customize the following configuration:
* install.version: The version of elasticsearch to install
* elasticsearch.http.port: The port that Elasticsearch nodes will listen on for HTTP requests
* elasticsearch.tcp.port: The port that Elasticsearch nodes will listen on for TCP requests
* cluster.initial.size: The number of elasticsearch nodes to create
