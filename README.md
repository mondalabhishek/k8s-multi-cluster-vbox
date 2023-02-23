PS: Don't hesitate  give a star ⭐ :heart: :star: 

# k8s-cluster

spin up three node cluster

* 192.168.56.2 master
* 192.168.56.4 worker-1
* 192.168.56.6 worker-2

see the corresponding post from [here](https://baykara.medium.com/setup-own-kubernetes-cluster-via-virtualbox-99a82605bfcc)

* requirements
```
vagrant
virtualbox
```

Fire up vms
``` 
vagrant up
```
To access each machine respectively via 
```
vagrant ssh master
```
in master node

```
1. set root password
2. switch root account
3. kubeadm init --apiserver-advertise-address 192.168.33.13 --pod-network-cidr=10.244.0.0/16
4. remove --port 0 from /etc/kubernetes/manifests/kube-[controller-api| scheduler].yaml
5. join workers to master node
```
for workers
```
vagrant ssh [worker1|worker2]
```
* Post VM Creation:
1. vagrant ssh master
	- sudo su root
	- kubeadm init --apiserver-advertise-address 192.168.56.2 --pod-network-cidr=10.244.0.0/16
	- note the join cmd output.
	- logout as root
	- create kube config file:
		-- mkdir -p $HOME/.kube
 		-- sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
		-- sudo chown $(id -u):$(id -g) $HOME/.kube/config
	- Install network plugin:
		kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
	- Wait for Scheduele, ContollerManager, etcd to be ready - kubectl get cs
	- Wait for master node to be ready: kubectl get nodes
	
2. Now ssh into both worker
	- Execute the join cmd as root
3. ssh into master and check if all worker nodes are ready or not : kubectl get nodes