# Create a Gzipped Tar file of a directory
## @tags: tar, compression
`tar -czvf dir-1.tar.gz dir-1`

# Extract the content of a Gzipped Tar file
## @tags: tar, compression
`tar -xzvf dir-1.tar.gz`

# Check what is listening on port 22
## @tags: linux, port
`sudo ss -lp "sport = :22"`

# Run a docker image with shell
## @tags: docker, shell
`docker run -it --rm --entrypoint sh alpine/git:1.0.30`

# Run a docker image with shell in a Kubernetes cluster
## @tags: kubernetes, k8s, docker, shell
`kubectl run python --namespace=my-namespace --tty -i --image=python:alpine3.15 bash`

# Run a docker image with shell in a Kubernetes cluster and re-attach it later on
## @tags: kubernetes, k8s, docker, shell
- `kubectl run postgres --namespace=my-namespace -t -i --restart=Never --image=postgres:13.0-alpine bash`
- `kubectl attach postgres --container=postgres -t -i --namespace=my-namespace`

# checksum
## @tags: sha, security
1. `shasum -a 512 <file>`
2. `shasum --check`

# Export environment variables in a .env file
## @tags: env, envvar, .env
- `export $(xargs < .env)`
- ignoring commented lines: `export $(grep -v '^#' .env | xargs)`

# Decode a JWT token
## @tags: jwt, token
```bash
export JWT="eyJH...."
jq -R 'split(".") | .[0],.[1] | @base64d | fromjson' <<< $(echo "${JWT}")
```
source: https://gist.github.com/angelo-v/e0208a18d455e2e6ea3c40ad637aac53

# Combine kubeconfig files in one
## @tags: kubernetes, k8s, config, kubeconfig
`KUBECONFIG=~/.kube/config-1:~/.kube/config-2 kubectl config view --flatten > ~/.kube/config`

# Extract a kubeconfig from a merged kubeconfig
## @tags: kubernetes, k8s, config, kubeconfig
`KUBECONFIG=$HOME/.kube/config kubectl config view --minify --flatten --context=my- context > $HOME/.kube/my-context-config`

# Check if a tool is installed otherwise install it
## @tags: cli, apt, linux
`command -v git >/dev/null 2>&1 || { apt-get -qq update -y && apt-get -qq install -y git; }`

# Install nginx ingress controller on Docker desktop Kubernetes
## @tags: kubernetes, k8s, helm, nginx
`helm upgrade --install ingress-nginx ingress-nginx  --repo https://kubernetes.github.io/ingress-nginx  --namespace ingress-nginx --create-namespace`

# Create a file without vi
## @tags: vi
1. `cat << STOP > the-file`
2. end the editing by typing `STOP`

# MacOS: backup all applications installed with brew
## @tags: macos, brew, backup
1. dump: `brew bundle dump`
2. this will create a text file called `Brewfile`

# MacOS: add user to the sudoer list
## @tags: macos, sudo, sudoer
1. open the folder `/etc/` with Finder: from the 'Go' menu navigate down to 'Go to Folder'
2. show the info of the `sudoers` file in this folder: `Command + I`
3. change the permission of this file for everyone to 'read & write': click on the Lock icon and change the permission
4. edit the `/etc/sudoers` file add the line `username ALL=(ALL) ALL` after the line `root ALL=(ALL) ALL`
5. revert the change of the permission of the file above

# MacOS: setup terminal to use the fingerprint for sudo password
## @tags: macos, password, sudo, fingerprint
1. open the folder `/etc/pam.d/` with Finder: from the `Go` menu navigate down to `Go to Folder`
2. show the info of the `sudo` file in this folder: `Command + I`
3. change the permission of this file for everyone to `read & write`: click on the Lock icon and change the permission
4. edit the `/etc/pam.d/sudo` file add the line `auth sufficient pam_tid.so` at the top
5. revert the change of the permission of the file above
6. test with `sudo ls -l`

# Check healtcheck output of a docker container in docker-compose
## @tags: docker, container, docker-compose, jq
`docker inspect --format "{{json .State.Health }}" <my-container> | jq`

# Export variables in a .env file
## @tags: env, envvar, .env, xargs
- `while read -r line; do  export "$line"; done < .env`
- `export $(xargs < .env)`

# Create a random password
## @tags: password, random, base64, openssl
- without special characters(+=/): `date | shasum | base64| head -c 20; echo`
- with special characters(+=/): `openssl rand -base64 20`

# Clone a git repository with personal access token
## @tags: git, oauth, token
`git clone https://oauth2:<my-oauth-token>@github.com/username/my-repo.git`

# MacOS: libvirt
## @tags: macos, libvirt, brew, service
- To restart libvirt after an upgrade: `brew services restart libvirt`
- Or, if you don't want/need a background service you can just run: `/opt/homebrew/opt/libvirt/sbin/libvirtd -f /opt/homebrew/etc/libvirt/libvirtd.conf`

# Search in some typical files recursively using grep
## @tags: grep
`grep -iR proxy --include=my-file`

# Shell script check
## @tags: shell, test, check
https://www.shellcheck.net/

# Find files and execute a command on them
## @tags: find
`find . -name '*.yaml' -exec grep namespace {} \;`

# Check the correctness of Kustomize YAML files
## @tags: kubernetes, k8s, kubectl, kustomize
`kubectl kustomize overlays/prod/ --output=test.yaml`

# Generate a Kubernetes resource manifest from a deployed resource
## @tags: kubernetes, k8s, kubectl, deploy, jq
```bash
kubectl get pods db-tool-69ccf596bc-xgs2m -n my-namespace -o json \
| jq 'del(.metadata["namespace","creationTimestamp","resourceVersion","selfLink","uid","annotations","ownerReferences","status"])'
```

# Start a Podman VM
## @tags: vm, podman
`podman machine init --now --cpus 4 --memory=8192 --disk-size=128`

# MacOS: start libvirt without a service
## @tags: macos, libvirt, service
`/opt/homebrew/opt/libvirt/sbin/libvirtd -f /opt/homebrew/etc/libvirt/libvirtd.conf`

# Env variable in env variable name
## @tags: env, envvar
`export MY_VAR=MY_VAR_${MY_PARAM_VAR^^}`

# MacOS: LIMA exec format error
## @tags: macos, lima, platform, arm, amd
Platform "linux/amd64" seems incompatible with the host platform "linux/arm64/v8".
If you see "exec format error", see https://github.com/containerd/nerdctl/blob/main/docs/multi-platform.md

# Make sure JAVA_HOME is set correctly
## @tags: java, env, envvar
`export JAVA_HOME="$(dirname $(dirname $(realpath $(which javac))))"`

# If git repo command with SSH does not work
## @tags: git, ssh
Check the output of 'ssh -T git@gitlab.com' to see which user is being used

# Build Docker image for multiple platform with buildx
## @tags: docker, build, platform, amd, arm, amd
`docker buildx build --push --platform linux/arm/v7,linux/arm64/v8,linux/amd64 -t image:image_tag .`
