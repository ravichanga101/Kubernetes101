# Kubernetes101
Learning

kubectl get pods -n dev
kubectl get pods -A
kubectl get all -A
kubectl run nginx --image=nginx --labels=env=pro --dry-run=client -o yaml > nginx_pod.yaml
kubectl delete pod nginx --grace-period 0 --force
kubectl create deploy --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
kubectl expose pod valid-pod --port=444 --name=frontend
kubectl replace --force -f nginx.yaml
kubectl edit deploy nginx
kubectl set image deploy nginx nginx=nginx:1.18
kubectl scale deploy nginx --repliacs=4 --record
kubectl get events

## Scheduling
kubectl get pods --show-labels
kubectl get pods -l env=dev
kubectl describe node node01 | grep -i Taints:
kubectl label nodes node01 size=small

/etc/kubernetes/manifests ---> static POD Definition

kubectl logs nginx
kubectl logs <pod_name> -c <container_name>

## Monitoring

kubectl top pod --containers=true

## Application Lifecycle Management

kubectl rollout status deployment/app
kubectl rollout history deployment/app
kubectl rollout undo deployment/app

kubectl create configmap app-config --from-literal=env=dev
kubectl create secret generic app-secret --from-literal=pass=123

## Cluster Maintenance

kubectl drain node01
kubectl uncordon node01


kubectl create serviceaccount sa_1
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
kubectl certificate approve john

kubectl config view

kubectl config current-context
kubectl config use-context prod-user@production
kubectl auth can-i create deployments --as john

kubectl create role dev --verb=create --resource=secret

kubectl create rolebinding dev-john --role dev --user john
kubectl api-resources --namespaced=true


## Troubleshooting

kubectl get pods -n kube-system

kubectl get nodes

df -h
top

systemctl status kubelet
sudo journalctl -u kubelet

ps -aux | grep kubelet
kubectl cluster-info

## Find pod CIDR:
kubectl describe node | less -p PodCIDR
kubectl get pod -A --sort-by=.metadata.creationTimestamp

kubectl get events -A --sort-by=.metadata.creationTimestamp

## Find the service CIDR of node-master:
ssh node0master
cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep range

# Find which CNI plugin is used on node-master:
ls /etc/cni/net.d/

Find internal IP of all nodes:
kubectl get nodes -o jsonpath=’{.items[*].status.addresses[?(@.type==”InternalIP”)].address}’

kubectl create --dry-run --validate -f <file>.yaml

kubectl run --rm -it --image=<image> <podname> -- sh

kubectl create namespace <namespace-name>
kubectl get ns dev -o yaml
kubectl exec --stdin --tty ubuntu -- sh

kubectl exec ubuntu -- env
kubectl explain <resource>
kubectl get <resource> --sort-by=.metadata.name

kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml
kubectl run nginx --image=nginx --restart=Never --limits='cpu=300m,memory=512Mi' --dry-run=client -o yaml
kubectl run nginx --image=nginx --restart=Never --requests='cpu=100m,memory=256Mi' --limits='cpu=300m,memory=512Mi' --dry-run=client -o yaml
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml >nginx-pod.yaml

kubectl create deploy nginx --image=nginx  --replicas=3 --dry-run=client -o yaml

kubectl create deploy nginx --image=nginx  --replicas=3 --dry-run=client -o yaml >nginx-deployment.yml
kubectl apply -f nginx-deployment.yml

kubectl expose deployment nginx --port=80 --type=ClusterIP
kubectl expose deployment nginx --port=80 --type=NodePort
$ kubectl expose pod redis  --type=ClusterIP --port=6379 --target-port=6379
kubectl scale --replicas=3 deployment nginx

kubectl drain  <node-name> --delete-local-data --ignore-daemonsets
kubectl cordon <node-name>
kubectl uncordon <node-name>

kubectl rollout undo deployment/web --to-revision=3
$ kubectl label nodes <node-name> <label-key>=<label-value>

kubectl cp myfile.txt mypod1:/myfile.txt


https://k8s.work/cka-lab.mp4

https://kube.academy/courses/how-to-prepare-for-the-cka-exam/lessons/the-curriculum




