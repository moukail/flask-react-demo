OpenShift
=========
```bash
curl -L https://developers.redhat.com/content-gateway/file/pub/openshift-v4/clients/crc/2.59.0/crc-linux-amd64.tar.xz | tar -xJ

export KUBECONFIG=~/.crc/machines/crc/kubeconfig

sudo dnf install bash-completion
echo "source <(crc completion bash)" >> ~/.bashrc
source <(crc completion bash)

systemctl --user restart crc-daemon.service crc-http.socket crc-vsock.socket

### Delete CRC cluster
crc delete -f
### or use virsh
sudo virsh list

virsh --connect qemu:///system list
virsh --connect qemu:///system domifaddr crc
virsh --connect qemu:///system console crc
virsh --connect qemu:///system domdisplay crc
virsh --connect qemu:///system destroy crc
virsh --connect qemu:///system undefine crc

### https://github.com/crc-org/engineering-docs/blob/main/content/Debugging.md
ssh -i ~/.crc/machines/crc/id_ed25519 -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -p 2222 core@127.0.0.1

### Setup CRC cluster
sudo modprobe kvm_intel || true
crc cleanup
crc setup
crc config set disk-size 60
crc start
crc console
crc ip
oc whoami --show-console

oc config view
oc login -u developer https://api.crc.testing:6443
oc login -u kubeadmin -p <PASSWORD> <API_URL>
oc whoami -t
oc get clusteroperators

oc whoami -t

### Create a new project
oc new-project flask-react
oc project python-test

oc create secret docker-registry docker-secret --docker-server=docker.io --docker-username=<USERNAME> --docker-password=<PASSWORD> --docker-email=<EMAIL>

### Create a secret for postgresql
oc create secret generic postgresql-secret --from-literal=POSTGRES_PASSWORD=pass_1234 --from-literal=POSTGRES_USER=admin
oc get secret postgresql-secret -o yaml

### Create a configmap for postgresql
oc create configmap postgresql-config --from-literal=POSTGRES_DB=demo_db
oc get configmap postgresql-config -o yaml

### Create a deployment for postgresql
oc new-app --image=docker.io/library/postgres:18 --name=postgres
oc set env deployment/postgres --from=secret/postgresql-secret
oc set env deployment/postgres --from=configmap/postgresql-config
oc get deployment postgres -o yaml | grep -A 15 env | head -n 20

oc explain deployment.spec.template.spec.containers

### replicaset
oc get rs 
oc delete rs postgres-7cb8fb5699 && oc get pods

### Troubleshooting
oc describe pods postgres | grep "FailedScheduling" -A 5
oc delete pods --field-selector status.phase=Failed
oc delete pods --field-selector status.phase=Succeeded

oc get nodes -o custom-columns=NAME:.metadata.name,TAINTS:.spec.taints
oc adm prune images --confirm

oc get pods
oc events --types=Warning --for pod/postgres-bd58c5fbd-vkqhw
oc logs postgres-bd58c5fbd-vkqhw

### delete postgresql
oc delete deployment,service postgres

### Create an image stream for python
oc explain imagestream
oc create imagestream python
oc create imagestreamtag python:3.14-alpine3.23 --from-image=docker.io/library/python:3.14-alpine3.23

oc new-app --context-dir=backend --name=flask-react-backend https://github.com/moukail/flask-react-demo.git
oc new-app --image=docker.io/library/node:25.8-alpine3.23 --context-dir=frontend --name=flask-react-frontend https://github.com/moukail/flask-react-demo.git

oc get pods
oc get svc
oc get route

oc --context crc-admin get nodes -o custom-columns=NAME:.metadata.name,TAINTS:.spec.taints
oc --context crc-admin adm prune images --confirm --keep-tag-revisions=1
oc --context crc-admin delete pods -A --field-selector=status.phase=Failed
oc --context crc-admin delete pods -A --all --force --grace-period=0
oc --context crc-admin describe node crc | grep -A 5 DiskPressure

```

```bash
# Create a new project
oc new-project flask-react

# Create a new application from the Containerfile
oc new-app Containerfile --name flask-react-backend

# Expose the application
oc expose svc flask-react-backend

# Get the URL of the application
oc get route flask-react-backend
```

#### Installing Tekton and Shipwright
```bash
kubectl apply --filename https://infra.tekton.dev/tekton-releases/pipeline/latest/release.yaml
#kubectl apply -f https://operatorhub.io/install/shipwright-operator.yaml
kubectl apply -f https://github.com/shipwright-io/build/releases/latest/download/release.yaml --server-side=true
```