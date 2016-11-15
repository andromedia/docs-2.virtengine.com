---
title: VirtEngine
order: 1
requirements:
  build: VirtEngine
  plan: free
---

In order to faciliate to use our products the following informations are useful:

1. When you are using a single server for effecting all operations the  default configuration suffices.

2. When you are in a position to use separate servers to run VirtEngine or encounter failures while running, the following configurations are to be changed.

The following are the main components which are to be changed:

- *Console (UI - Nilavu)*
- *API - gateway*
- *Omni scheduler - VirtEngine*

### Changing components

The main options to change are in the following files:

1. Change details for the UI               */var/lib/megam/nilavu.conf*
2. Change details for the API              */var/lib/megam/VirtEnginegateway/gateway.conf*
3. Change details for the Omni scheduler   */var/lib/megam/VirtEngine/VirtEngine.conf*

Go to

```bash

$ cd /var/lib/megam

```

Configure */var/lib/megam/nilavu.conf*
{: .info}

*/var/lib/megam/site_settings.yaml* file is self explanatory, tweak as you wish.

*/var/lib/megam/nilavu.conf*

~~~yaml

## api host that the UI will connect to

http_api = http://localhost:9000/v2

## log streamer that the UI will connect to.

log_server = ws://localhost:7777/logs

### vnc server that the UI will connect to.

vnc_server = ws://localhost:8000

~~~

Configure */var/lib/megam/VirtEnginegateway/gateway.conf*
{: .info}


~~~yaml

# We use  cassandra as the db layer
# ~~~~~

# Cassandra
# ~~~~~~~~~
cassandra.host = "localhost"
cassandra.keyspace = "VirtEngine"
# DON'T Change these
cassandra.username = "vertadmin"
cassandra.password = "vertadmin"
cassandra.use_ssl = false

~~~

~~~yaml

# We use NSQ as the messaging layer
# ~~~~~
nsq.url="http://localhost:4151"

# Don't change the nsq.topic names.
nsq.topic.vms="vms"
nsq.topic.containers="containers"

# send messages to NSQ.
nsq.events.muted = false

# don't send events to NSQ if the email id is tour@virtengine.com
nsq.events.muted_emails = ["tour@virtengine.com"]

~~~

Configure */var/lib/megam/VirtEngine/VirtEngine.conf*
{: .info}


~~~yaml

### Welcome to the VirtEngine configuration file.
###
### [meta]
###
### Controls how VirtEngine connects to scylla, nsq

  [meta]
    api = "https://localhost:9000/v2"
    nsqd = ["localhost:4150"]
    scylla = ["localhost"]
    scylla_keyspace = "VirtEngine"
    scylla_username = "vertadmin"
    scylla_password = "vertadmin"

~~~
