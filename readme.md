# Client setup

## start `k8s-cluster`
```
vagrant up
```


## update in `/etc/hosts` file
```
192.168.0.90 kmaster.example.com kmaster
192.168.0.91 kworker1.example.com kworker1
192.168.0.92 kworker2.example.com kworker2
```
## download latest `kubectl` from internet
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

## config `config` from kmaster
```
rm -rf ${HOME}/.kube
mkdir ${HOME}/.kube
scp vagrant@kmaster:.kube/config ${HOME}/.kube/
```
## How to verify cluster running status
```
~> kubectl get nodes -o wide
NAME                   STATUS   ROLES    AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE                KERNEL-VERSION              CONTAINER-RUNTIME
kmaster.example.com    Ready    master   44m   v1.13.4   192.168.0.90   <none>        CentOS Linux 7 (Core)   3.10.0-957.5.1.el7.x86_64   docker://18.9.3
kworker1.example.com   Ready    <none>   42m   v1.13.4   192.168.0.91   <none>        CentOS Linux 7 (Core)   3.10.0-957.5.1.el7.x86_64   docker://18.9.3
```
## running busybox container in k8s-cluster
```
:~$ kubectl run myshell --rm -it --image busybox -- sh
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
If you don't see a command prompt, try pressing enter.
/ # 
/ # 
/ # ping google.com
PING google.com (172.217.163.78): 56 data bytes
64 bytes from 172.217.163.78: seq=0 ttl=61 time=2.702 ms
64 bytes from 172.217.163.78: seq=1 ttl=61 time=2.783 ms
^C
--- google.com ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 2.702/2.742/2.783 ms
/ # 
```
