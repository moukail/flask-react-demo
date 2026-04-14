#### install odo
```bash
curl -L https://developers.redhat.com/content-gateway/rest/mirror/pub/openshift-v4/clients/odo/v3.16.1/odo-linux-amd64 -o odo
curl -L https://developers.redhat.com/content-gateway/rest/mirror/pub/openshift-v4/clients/odo/v3.16.1/odo-linux-amd64.sha256 -o odo.sha256
echo "$(<odo.sha256)  odo" | shasum -a 256 --check
sudo install -o root -g root -m 0755 odo /usr/local/bin/odo

odo version
odo login
```
#### add registry
```bash
odo preference view
odo preference add registry StageRegistry https://registry.stage.devfile.io
odo preference set ImageRegistry quay.io
odo registry
```

#### create project
```bash
mkdir quickstart-demo && cd quickstart-demo
npm install express --save
npx express-generator

oc login -u developer https://api.crc.testing:6443
odo create project flask-react
odo init
odo init --name backend --devfile python \
  --devfile-registry DefaultDevfileRegistry --devfile-version 2.2.0

odo dev
odo dev --build-command install
odo logs --follow
odo deploy
odo describe component

odo delete namespace odo-dev
```
