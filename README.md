# etcd experiment

```
-- boot up 3 nodes --
$ vagrant up

-- from each node (after ssh ubuntu@192.168.33.11 or ubuntu@192.168.33.12 or ubuntu@192.168.33.13) --
$ source /vagrant/etcd_env
$ export THIS_NAME=${NAME_1} # NAME_2, NAME_3 for other nodes
$ export THIS_HOST=${HOST_1} # HOST_2, HOST_3 for other nodes
$ docker run -d --net=host --name etcd quay.io/coreos/etcd:${ETCD_VERSION} \
    /usr/local/bin/etcd                                                  \
      --data-dir=data.etcd                                               \
      -name ${THIS_NAME}                                                 \
      -initial-advertise-peer-urls http://${THIS_HOST}:2380              \
      -listen-peer-urls http://${THIS_HOST}:2380                         \
      -advertise-client-urls http://${THIS_HOST}:2379                    \
      -listen-client-urls http://${THIS_HOST}:2379,http://localhost:2379 \
      -initial-cluster ${CLUSTER}                                        \
      -initial-cluster-token ${CLUSTER_TOKEN}                            \
      -initial-cluster-state ${CLUSTER_STATE}

-- from whichever node --
# add value to key 'message'
$ curl -L http://localhost:2379/v2/keys/message -XPUT -d value="Hello world"
{"action":"set","node":{"key":"/message","value":"Hello world","modifiedIndex":9,"createdIndex":9}}

# read value with key 'messsage'
$ curl -L http://localhost:2379/v2/keys/message
{"action":"get","node":{"key":"/message","value":"Hello world","modifiedIndex":9,"createdIndex":9}}

# wait for value to change
$ curl -L http://localhost:2379/v2/keys/message?wait=true
```

# References

- https://coreos.com/etcd/docs/latest/clustering.html
- https://coreos.com/etcd/docs/latest/docker_guide.html
- https://github.com/coreos/etcd/blob/v3.0.1/Documentation/op-guide/container.md#docker
- https://coreos.com/etcd/docs/latest/api.html
