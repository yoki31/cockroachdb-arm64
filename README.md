![CockroachDB](https://raw.githubusercontent.com/cockroachdb/cockroach/master/docs/media/cockroach_db.png)

# CockroachDB for ARM64 (v8)

This is a recent-edition build of CockroachDB for ARM64 devices, namely machines like RaspberryPI and PINE64 and similar boards with the ARM64v8 chipset.

## Docker Hub image available
[Docker Hub](https://hub.docker.com/r/christoofar/cockroachdb-arm64/tags?page=1&ordering=last_updated) has the latest built image.

## Running image (single instance with 20% memory cache)

Note: This assumes you have a docker network named "proxy" and you're likely creating this on Docker Swarm/k8s.
Adjust the docker service create statement to fit your environment/purpose.

```
sudo mkdir /var/lib/cockroach
sudo mkdir /var/lib/cockroach/data
sudo mkdir /var/lib/cockroach/backup

docker service create --replicas 1 --name cockroachdb-1 --hostname cockroachdb-1 --network proxy \
                --mount type=bind,source=/var/lib/cockroach/data,target=/cockroach/cockroach-data \
                --mount type=bind,source=/var/lib/cockroach/backup,target=/cockroach/backup --stop-grace-period 60s \
                --publish 8090:8080 --publish 26257:26257 christoofar/cockroachdb-arm64 start-single-node \
                --storage-engine=pebble --external-io-dir=/cockroach/backup --cache=.20 --max-sql-memory=.20 --logtostderr \
                --insecure
```
