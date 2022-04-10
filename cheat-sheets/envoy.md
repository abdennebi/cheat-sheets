Ask the local envoy proxy what it knows about its listeners, routes, clusters.

curl localhost:15000
curl localhost:15000/listeners
curl localhost:15000/routes
curl localhost:15000/clusters
curl localhost:15000/server_info
curl localhost:15000/config_dump
