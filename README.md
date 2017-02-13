# Brooklyn Elasticsearch

A Pure Brooklyn YAML blueprint to deploy a cluster of [Elasticsearch](https://www.elastic.co/products/elasticsearch) nodes.

Currently you can customize the following configuration:
* install.version: The version of elasticsearch to install
* elasticsearch.http.port: The port that Elasticsearch nodes will listen on for HTTP requests
* elasticsearch.tcp.port: The port that Elasticsearch nodes will listen on for TCP requests
* cluster.initial.size: The number of elasticsearch nodes to create
* elasticsearch.user: The username of the root elastic search user
* elasticsearch.password: The password for the root elastic search user

To create a cluster of Elasic search nodes with a single load balanced http entry point, you can put an [NginX](https://www.nginx.com) proxy in front of the entity with a blueprint like this:

    location: my-location
    name: Elastic Search Cluster
    services:
    - type: elasticsearch
      id: elasticsearch
      name: Elastic Search Nodes
    - type: org.apache.brooklyn.entity.proxy.nginx.NginxController
      id: nginx
      name: Load Balancer (nginx)
      brooklyn.config:
        proxy.http.port: 9220
        loadbalancer.serverpool: $brooklyn:entity("elasticsearch")
        nginx.sticky: false
    brooklyn.enrichers:
    - type: org.apache.brooklyn.enricher.stock.Propagator
      brooklyn.config:
        producer: $brooklyn:entity("nginx")
        propagating:
        - main.uri
        - main.uri.mapped.subnet
        - main.uri.mapped.public
