# Kong API Gateway Example
Kong API gateway example service using vagrant, ansible.


## Service Architecture
See `Vagrantfile`

### `kong` node
Kong API gateway node.

- kong apigw
  - http: http://192.168.33.10:8000
  - http (admin): http://192.168.33.10:8001
  - https: https://192.168.33.10:8443
  - https (admin): https://192.168.33.10:8444

- konga ui: http://192.168.33.10:1337

### `nginx` node
Simple nginx server.
- nginx: http://192.168.33.11


## Test

### Prerequisites

- vagrant `1.9.1`
- ansible `2.4.1.0`

### Steps

1. Setup VMs
`kong`, `nginx` instance will be set up.

``` bash
# create vms, provision, orchestrates services simply.
$ vagrant up

 :

# check vms
$ vagrant status
Current machine states:

kong                      running (virtualbox)
service                   running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.

# check kong services (kong services run on docker)
$ vagrant ssh kong

 :

$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                                                                      NAMES
977c247182fd        pantsel/konga       "/app/start.sh"          2 hours ago         Up 2 hours          0.0.0.0:1337->1337/tcp                                                                     konga
09d6b09d57f9        kong:latest         "/docker-entrypoint.…"   2 hours ago         Up 2 hours          0.0.0.0:8000-8001->8000-8001/tcp, 0.0.0.0:8443-8444->8443-8444/tcp, 0.0.0.0:80->8000/tcp   kong
703ab553be86        postgres:9.4        "docker-entrypoint.s…"   2 hours ago         Up 2 hours          5432/tcp                                                                                   kong-database

```

2. Configure api gateway endpoint
- Access konga. http://192.168.33.10:1337
- Login as admin (username: `admin`, password: `adminadminadmin`)
- Define API gateway connection. (kong admin url: `http://kong:8001`)
- You need to set `nginx` endpoint manually.


## Provisioning & Orchestration codes
See `ansible/infrastructure.yml` & `ansible/apigw.yml`
