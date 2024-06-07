# PMACCT issue #768 Docker testbed

This repository contains a ready-to-go Docker testbed for PMACCT issue [768](https://github.com/pmacct/pmacct/issues/768).
You will require both Docker and Docker compose installed:
- https://docs.docker.com/engine/install/
- https://docs.docker.com/compose/install/

## PMACCT

We use the `nfacctd` daemon of PMACCT to aggregate NetFlow/IPFIX flows. All configuration is found in `nfacctd.conf`
with the BGP neighbors listed in `bgp.map`.

Once the container has been running for a few minutes the log output should look like:

```shell
nfacctd-1  | INFO ( default/core ): NetFlow Accounting Daemon, nfacctd 1.7.10-git (20240603-2 (f424904c))
nfacctd-1  | INFO ( default/core ):  '--enable-mysql' '--enable-pgsql' '--enable-sqlite3' '--enable-kafka' '--enable-geoipv2' '--enable-jansson' '--enable-rabbitmq' '--enable-nflog' '--enable-ndpi' '--enable-zmq' '--enable-avro' '--enable-serdes' '--enable-redis' '--enable-gnutls' 'AVRO_CFLAGS=-I/usr/local/avro/include' 'AVRO_LIBS=-L/usr/local/avro/lib -lavro' '--enable-l2' '--enable-traffic-bins' '--enable-bgp-bins' '--enable-bmp-bins' '--enable-st-bins'
nfacctd-1  | INFO ( default/core ): Reading configuration file '/etc/pmacct/nfacctd.conf'.
nfacctd-1  | INFO ( default/core ): [/etc/pmacct/mappings/bgp.map] (re)loading map.
nfacctd-1  | INFO ( default/core ): [/etc/pmacct/mappings/bgp.map] map successfully (re)loaded.
nfacctd-1  | INFO ( default/core/BGP ): maximum BGP peers allowed: 1
nfacctd-1  | INFO ( default/core/BGP ): waiting for BGP data on interface=all ip=:: port=179/tcp
nfacctd-1  | INFO ( default/core ): waiting for NetFlow/IPFIX data on interface=all ip=:: port=5009/udp
nfacctd-1  | INFO ( TEST/print ): cache entries=16411 base cache memory=72208400 bytes
nfacctd-1  | INFO ( TEST/print ): JSON: setting object handlers.
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - START (PID: 10) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - END (PID: 10, QN: 68539/68539, ET: 0) ***
nfacctd-1  | INFO ( default/core/BGP ): [x.x.x.x] BGP peers usage: 1/1
nfacctd-1  | INFO ( default/core/BGP ): [x.x.x.x] Capability: MultiProtocol [1] AFI [1] SAFI [1]
nfacctd-1  | INFO ( default/core/BGP ): [x.x.x.x] Capability: 4-bytes AS [65] ASN [12345]
nfacctd-1  | INFO ( default/core/BGP ): [x.x.x.x] BGP_OPEN: Local AS: 12345 Remote AS: 12345 HoldTime: 90
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - START (PID: 11) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - END (PID: 11, QN: 115922/115922, ET: 0) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - START (PID: 12) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - END (PID: 12, QN: 116312/116312, ET: 0) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - START (PID: 13) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - END (PID: 13, QN: 115690/115690, ET: 0) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - START (PID: 14) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - END (PID: 14, QN: 116318/116318, ET: 0) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - START (PID: 15) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - END (PID: 15, QN: 116697/116697, ET: 0) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - START (PID: 16) ***
nfacctd-1  | INFO ( TEST/print ): *** Purging cache - END (PID: 16, QN: 114943/114943, ET: 0) ***
```

We can then check the JSON output found in `./data/1m_nfacctd.json`, the output created on our end is missing some data
as is described in issue #768:

```json
{"event_type": "purge", "as_dst": 0, "ip_dst": "x.x.x.137", "net_dst": "0.0.0.0", "mask_dst": 0, "stamp_inserted": "2024-06-07 09:21:00", "stamp_updated": "2024-06-07 09:22:01", "packets": 1, "bytes": 52}
{"event_type": "purge", "as_dst": 0, "ip_dst": "x.x.x.210", "net_dst": "0.0.0.0", "mask_dst": 0, "stamp_inserted": "2024-06-07 09:21:00", "stamp_updated": "2024-06-07 09:22:01", "packets": 1, "bytes": 1340}
{"event_type": "purge", "as_dst": 0, "ip_dst": "x.x.x.226", "net_dst": "0.0.0.0", "mask_dst": 0, "stamp_inserted": "2024-06-07 09:21:00", "stamp_updated": "2024-06-07 09:22:01", "packets": 1, "bytes": 52}
{"event_type": "purge", "as_dst": 0, "ip_dst": "x.x.x.164", "net_dst": "0.0.0.0", "mask_dst": 0, "stamp_inserted": "2024-06-07 09:21:00", "stamp_updated": "2024-06-07 09:22:01", "packets": 1, "bytes": 52}
{"event_type": "purge", "as_dst": 0, "ip_dst": "x.x.x.91", "net_dst": "0.0.0.0", "mask_dst": 0, "stamp_inserted": "2024-06-07 09:21:00", "stamp_updated": "2024-06-07 09:22:01", "packets": 1, "bytes": 1500}
{"event_type": "purge", "as_dst": 0, "ip_dst": "x.x.x.193", "net_dst": "0.0.0.0", "mask_dst": 0, "stamp_inserted": "2024-06-07 09:21:00", "stamp_updated": "2024-06-07 09:22:01", "packets": 1, "bytes": 1452}
{"event_type": "purge", "as_dst": 0, "ip_dst": "x.x.x.238", "net_dst": "0.0.0.0", "mask_dst": 0, "stamp_inserted": "2024-06-07 09:21:00", "stamp_updated": "2024-06-07 09:22:01", "packets": 1, "bytes": 1500}
{"event_type": "purge", "as_dst": 0, "ip_dst": "x.x.x.105", "net_dst": "0.0.0.0", "mask_dst": 0, "stamp_inserted": "2024-06-07 09:21:00", "stamp_updated": "2024-06-07 09:22:01", "packets": 1, "bytes": 1038}
{"event_type": "purge", "as_dst": 0, "ip_dst": "x.x.x.40", "net_dst": "0.0.0.0", "mask_dst": 0, "stamp_inserted": "2024-06-07 09:21:00", "stamp_updated": "2024-06-07 09:22:01", "packets": 1, "bytes": 1492}
{"event_type": "purge", "as_dst": 0, "ip_dst": "x.x.x.105", "net_dst": "0.0.0.0", "mask_dst": 0, "stamp_inserted": "2024-06-07 09:21:00", "stamp_updated": "2024-06-07 09:22:01", "packets": 2, "bytes": 3000}
```

## Getting up and running

### Managing the Docker container

Before starting the container make sure that the `bgp_ip` key in `bgp.map` has been populated with the correct IP
address. Once you have Docker installed, the `bgp.map` field populated with the correct IP, the container can be started
with:

```shell
~# docker compose up -d
```

To stop the container you can run:

```shell
~# docker compose down
```

### Logging

With the container running you can view the logs with:

```shell
~# docker compose logs nfacctd
```

To follow (tail) the logging of the container, add the `-f` flag, like this:

```shell
~# docker compose logs -f nfacctd
```

Note that the above only works when you are in the same directory as the `docker-compose.yml` file for this project, as
it utilises this to find the corresponding container(s). If you are not in the same directory you can still follow the
logs of a container. You will first need to do a `docker ps` to list the container(s):

```shell
~# docker ps
CONTAINER ID   IMAGE                          COMMAND                  CREATED         STATUS                  PORTS     NAMES
4a5702c3d8ef   pmacct/nfacctd:bleeding-edge   "/usr/local/sbin/nfa…"   6 seconds ago   Up 6 seconds                      pmacct-issue-768-docker-testdbed-nfacctd-1
```

Once you have the container name, in this case `pmacct-issue-768-docker-testdbed-nfacctd-1`, you can follow the logs
with:

```shell
docker logs -f pmacct-issue-768-docker-testdbed-nfacctd-1
```

In some cases a lot of logs are produced by a container, it can then be handy to only get the last number lines of the
output. To achieve this you can for instance add `--tail 50` to only get last 50 lines.

### Interact with container

It can be required to interact with the container for troubleshoot reasons, therefore it is possible to execute commands
inside the container or even enter the shell. You can execute a command like follows:

```shell
~# docker compose exec nfacctd cat /etc/pmacct/nfacctd.conf
```

To actually enter the container and interact with the shell you can run:

```shell
~# docker compose exec nfacctd bash
```

The same is again also possible by just using `docker` instead of `docker compose`. To enter the container however you
will need to provide the `-t` flag to allocate a pseudy TTY.

To exit the container you can use the escape sequence `CTRL + P` & `CTRL + Q`.

### Volume

The container makes use of volumes to mount the configuration files. A volume is also in place which is bound to the
output directory of the PMACCT daemon. This volume is mounted on the local host to `./data` in this project, all files
outputted by the daemon are found here.
