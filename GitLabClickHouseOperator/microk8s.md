## Install microk8s

https://microk8s.io/#install-microk8s

Edit ~/.zshrc and add `alias kubectl="microk8s kubectl"`

```
sudo snap install microk8s --classic
alias kubectl="microk8s kubectl"
sudo microk8s status --wait-ready
sudo usermod -a -G microk8s droscign
newgrp microk8s
microk8s enable dashboard
microk8s enable dns
microk8s enable registry
microk8s enable host-access
```
 
Take note of the newgrp command.  I am pretty sure that group management in Ubuntu is totally foobar.  I have to run the newgrp evey time I run
 a shell as the microk8s group is too far down in my group list.  I need to trim more grap out of my group list.
 
## Install golang
 
I had to remove first `sudo apt remove golang` and remove the env vars from my .zshrc (probably did not need to remove, just probably had to remove the badd env vars!
  then install `sudo apt install golang`
 
## Install the operator

https://gitlab.com/gitlab-org/opstrace/opstrace/-/blob/main/clickhouse-operator/README.md
 
```
cd ~/GitHub
git clone https://gitlab.com/gitlab-org/opstrace/opstrace.git
cd opstrace/clickhouse-operator
newgrp microk8s
make manifests build
microk8s.kubectl config view --raw > $HOME/.kube/config
kubectl apply -f ./config/crd/bases/clickhouse.gitlab.com_clickhouses.yaml
make run
```
 
### new tab
```
cd GitHub/opstrace/clickhouse-operator
newgrp microk8s
```
### Deploy a three node ClickHouse database

```
kubectl apply -f ./config/samples/example.yaml
microk8s kubectl get pods
```

## Queries

Find the IP Addr of the `example` service, `10.152.183.68` in my case, so it is in the example queries down below
```
kubectl get services
```
### Get the username and password
(admin and InsecurePassword or something like that)
```
cat config/samples/example.yaml
```

### List of databases
```
kubectl exec -it -n default statefulset/example-0 -- \
   clickhouse-client -u admin --password -h 10.152.183.68 \
   -f PrettyCompact -q 'SELECT name,engine,data_path FROM system.databases;'
```

### Cluster members
``` 
kubectl exec -it -n default statefulset/example-0 -- \
   clickhouse-client -u admin --password -h 10.152.183.68 \
   -f PrettyCompact \
   -q "SELECT cluster,shard_num,replica_num,host_name,host_address FROM system.clusters WHERE startsWith(host_name, 'example');"
```
