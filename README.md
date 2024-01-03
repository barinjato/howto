# MacOS utilities
## caffeinate - set Mac sleep behavior
### @tags: sleep, wait
- Running caffeinate with no flags or arguments prevents your Mac from going to sleep as long as the command continues to run.
- `caffeinate -u -t <seconds>` prevents sleep for the specified number of seconds.
- Adding the `-d` flag also prevents the display from going to sleep.
- Specifying an existing process with `-w <pid>` automatically exits the caffeinate command once the specified process exits.
- Passing a command with `caffeinate <command>` starts the given command in a new process and prevents sleep until that process exits.

## pbcopy, pbpaste - interact with system clipboard
### @tags: copy, paste
- `<command> | pbcopy` copies the output of the command to the clipboard.
- `pbpaste` outputs the contents of the clipboard to stdout.

## networkQuality - measure Internet speed
### @tags: network
- Run `networkQuality` to run an Internet speed test from your Mac.
- Add the `-v` flag to view more detailed information.
- Use the `-i` flag to run the network test on a specific network interface.

## sips - image manipulation
### @tags: image
- `sips -z <height> <width> <image>` resizes the specified image, ignoring the previous aspect ratio.
- `sips -Z <size> <image>` resizes the largest side of the specified image, preserving the aspect ratio.
- `sips -c <height> <width> <image>` crops the specified image to the given dimensions (relative to the center of the original image).
- `sips -r <degrees> <image>` rotates the image by the specified degrees.

By default, sips will destructively overwrite the input image. Use the -o flag to specify a different output file path (which must have the same file extension as the input image).

## open - open files and applications
### @tags: finder
- `open -a <app> <file>` opens the given file with the specified application.
- `open .` opens the current directory in a new Finder window.

## textutil - document file converter
### @tags: text, conversion
- `textutil` can convert files to and from Microsoft Word, plain text, rich text, and HTML formats.
- `textutil -convert html journal.doc` converts journal.doc into journal.html.
The possible values for `-convert` are: `txt, html, rtf, rtfd, doc, docx`.

## mdfind, mdls - search with Spotlight
### @tags: find
- `mdfind <query>` performs a keyword-based Spotlight search with the given query.
- `mdfind kMDItemAppStoreHasReceipt=1` finds all apps installed from the Mac App Store.
- `mdfind -name <name>` searches for all files matching the given name.
- The `-onlyin <dir>` flag restricts the search to the given directory.
- `mdls <file-path>` prints all Spotlight metadata associated with the given file.

## screencapture - take screenshots
### @tags: screen
- `screencapture -c` takes a screenshot and copies it to the clipboard.
- `screencapture <file>` takes a screenshot and saves it to the given file.
- Add the `-T <seconds>` flag to take the screenshot after the given delay in seconds.

## taskpolicy - control scheduling of processes
### @tags: task, scheduling
- `taskpolicy -b <command>` starts executing the given command in the background. On Apple silicon Macs, the process will only run on the efficiency cores.
- `taskpolicy -b -p <pid>` downgrades an existing process to run in the background.
- `taskpolicy -B -p <pid>` removes the specified process from running in the background. On Apple silicon Macs, the process may now run on the efficiency or performance cores. Note that this only works on processes that have been downgraded to the background, and not processes that started in the background.
- `taskpolicy -s <command>` starts the given command in the suspended state. This is useful to allow a debugger to attach to the process right at the start of execution.

## say - text-to-speech engine
### @tags: speech
- `say <message>` announces the given message.
- `say -f input.txt -o output.aiff` creates an audiobook from the given text file.

## pmset - configure power management
### @tags: power
- `pmset -g` prints all available power configuration information.
- `pmset -g assertions` displays information about power-related assertions made by other processes. This can be useful for finding a process that is preventing your Mac from going to sleep.
- `pmset -g thermlog` displays information about any processes that have been throttled (useful when running benchmarks).
- `pmset displaysleepnow` immediately puts the display to sleep without putting the rest of the system to sleep.
- `pmset sleepnow` immediately puts the entire system to sleep.

## networksetup - configure network settings
### @tags: network
- `networksetup -listnetworkserviceorder` prints a list of available network services.
- `networksetup -getinfo <networkservice>` prints information about the specified network service.
- `networksetup -getdnsservers <networkservice>` prints the DNS servers for the specified network service.
- `networksetup -setairportnetwork <device> <network> [password]` joins the specified Wi-Fi network. (In most cases, the `<device>` argument should be `"en0"`.)

## qlmanage - manage Quick Look
### @tags: 
- `qlmanage -p <file>` opens a Quick Look preview window of the given file.
- `qlmanage -m` prints status information about the Quick Look server process.
- `qlmanage -r` restarts the Quick Look server process.
- `qlmanage -r` cache resets the Quick Look thumbnail cache.

## softwareupdate - manage OS updates
### @tags: update
- `softwareupdate --list` prints out available software updates.
- `sudo softwareupdate -ia` installs all available updates.
- `softwareupdate --fetch-full-installer --full-installer-version <version>` tries to download the full installer of the specified macOS version to /Applications.

## system_profiler - view system information
### @tags: system
- `system_profiler` by default prints all available system information, which is usually overwhelming.
- `system_profiler <datatype>` only prints information about the given sub-system.
- `system_profiler -listDataTypes` lists all available sub-systems to get information from.

Some particularly useful ones:
- `system_profiler SPHardwareDataType` prints an overview of the hardware of the current machine, including its model name and serial number.
- `system_profiler SPSoftwareDataType` prints an overview of the software of the current machine, including the exact macOS version number.
- `system_profiler SPPowerDataType` prints power and battery information, including the current AC wattage and battery cycle count.
- `system_profiler SPDeveloperToolsDataType` prints the currently active version of the Xcode developer tools and SDK.

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
`KUBECONFIG=$HOME/.kube/config kubectl config view --minify --flatten --context=my-context > $HOME/.kube/my-context-config`

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

# Replacement using sed and environment variable whose value contains the '&'(ampersand) character that needs to be escaped, '&' has a special meaning in sed
## @tags: replace, sed, &, ampersand, env, envvar
`sed "s|replace_this|${WITH_THIS_ENV_VAR//&/\\&}|g" file`

# MacOS: OCR to extract text from an image file using the built-in shortcut app
## @tags: macos, ocr, image, text, shortcut
## @source: https://blog.greg.technology/2024/01/02/how-do-you-ocr-on-a-mac.html
The idea is roughly to use the MacOS `Shortcut` app with the following actions:
- `Extract Text from Image`
- Use `Shortcut Input`
- `Copy to Clipboard`

Run with:
- `shortcuts run <name of the shortcut> -i <image file path>`
- or using a python script:
```python
import subprocess

file_path = '<image file path>'
ocr_out = subprocess.check_output(
    f'shortcuts run ocr-text -i "{file_path}"', shell=True
)
print(ocr_out)
```

  

