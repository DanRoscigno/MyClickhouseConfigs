## Install microk8s
 
```
sudo snap install microk8s --classic
sudo microk8s status --wait-ready
microk8s enable dashboard dns registry
sudo usermod -a -G microk8s droscign
newgrp microk8s
```
 
Take note of the newgrp command.  I am pretty sure that group management in Ubuntu is totally foobar.  I have to run the newgrp evey time I run
 a shell as the microk8s group is too far down in my group list.  I need to trim more grap out of my group list.
 
## Install golang
 
I had to remove first `sudo apt remove golang` and remove the env vars from my .zshrc (probably did not need to remove, just probably had to remove the badd env vars!
  then install `sudo apt install golang`
 
## Install the operator
 
```
cd ~/GitHub
git clone https://gitlab.com/gitlab-org/opstrace/opstrace.git
cd opstrace/clickhouse-operator
make manifests build
alias kubectl="microk8s kubectl"
newgrp microk8s
microk8s.kubectl config view --raw > $HOME/.kube/config
make run
```
 
### new tab
```
cd GitHub/opstrace/clickhouse-operator
newgrp microk8s
microk8s kubectl apply -f ./config/samples/example.yaml
microk8s kubectl get pods
```
