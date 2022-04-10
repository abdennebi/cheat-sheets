
### List Services
``curl http://172.17.4.101:8500/v1/catalog/services``

```
{
    "consul": [],
    "fabio": [],
    "hello-world": [
        "urlprefix-hello-world.service/"
    ],
    "nomad": [
        "http",
        "rpc",
        "serf"
    ],
    "nomad-client": [
        "http"
    ]
}
```
### Service Details
``http://172.17.4.101:8500/v1/catalog/service/hello-world``

```
[
    {
        "ID": "9fd8eb78-7b46-ceb0-6c2c-da991ddd6aa9",
        "Node": "w1",
        "Address": "172.17.4.201",
        "Datacenter": "dc1",
        "TaggedAddresses": {
            "lan": "172.17.4.201",
            "wan": "172.17.4.201"
        },
        "NodeMeta": {},
        "ServiceID": "_nomad-executor-e1f0e43e-59b9-e1ea-33af-05e1caae7f9b-hello-world-hello-world-urlprefix-hello-world.service/",
        "ServiceName": "hello-world",
        "ServiceTags": [
            "urlprefix-hello-world.service/"
        ],
        "ServiceAddress": "10.0.2.15",
        "ServicePort": 8080,
        "ServiceEnableTagOverride": false,
        "CreateIndex": 560,
        "ModifyIndex": 560
    },
    {
        "ID": "e8ab698a-b296-0da5-a2d0-b5a0df8d15c5",
        "Node": "w2",
        "Address": "172.17.4.202",
        "Datacenter": "dc1",
        "TaggedAddresses": {
            "lan": "172.17.4.202",
            "wan": "172.17.4.202"
        },
        "NodeMeta": {},
        "ServiceID": "_nomad-executor-b695c62a-bcb8-5781-c213-0a02071629e5-hello-world-hello-world-urlprefix-hello-world.service/",
        "ServiceName": "hello-world",
        "ServiceTags": [
            "urlprefix-hello-world.service/"
        ],
        "ServiceAddress": "10.0.2.15",
        "ServicePort": 8080,
        "ServiceEnableTagOverride": false,
        "CreateIndex": 562,
        "ModifyIndex": 562
    }
]
```

### List Datacenters

``http://172.17.4.101:8500/v1/catalog/datacenters``

### List Nodes

``http://172.17.4.101:8500/v1/catalog/nodes``


### Deregister a service
``curl -XPUT 127.0.0.1:8500/v1/agent/service/deregister/<service-name>``

### Follow the logs of a running Consul agent
``consul monitor -log-level trace``

https://www.consul.io/docs/commands/monitor.html

### Healthcheck a service
``curl -s -S 127.0.0.1:8500/v1/health/service/account-service | jq .``

