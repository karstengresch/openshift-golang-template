## How to:
1. Login with Developer account to OpenShift:
```
# oc login --insecure-skip-tls-verify -u <username> https://openshift.example.com:8443
```

2. Create new project:
```
# oc new-project project1
```

3. Select `project1` project:
```
# oc project project1
```

4. Create ImageStream:
```
# oc create -f ./openshift-golang-template.yaml
```

5. Login to OpenShift console using browser (eg https://openshift.example.com:8443) with Developer account

6. Open `project1` project

7. Click `Add to Project` and select `Go`

8. Select `1.8.3 - latest` from drop down list and click `Select`

9. Set `Name` to `golang1`

10. Set `Git Repository URL` to `https://github.com/amsokol/openshift-golang-template.git` or click `Try It`

11. Click `advanced options` to add additional configuration params

12. Set `Context Dir` to `/example`

13. Add the following environment variables to `Build Configuration` section:
- GOPROJECT_ROOT=github.com/amsokol/openshift-golang-template/example
- GOPROJECT_CMD=cmd/server

```
GOPROJECT_ROOT tells builder the root package of go project
GOPROJECT_CMD tells builder the where "main()" function of "main" package to build and run is located (relative to GOPROJECT_ROOT).
Note: ignore GOPROJECT_CMD if "main()" function of "main" package is located in GOPROJECT_ROOT folder. 

In example above "main()" function of "main" package is located in `github.com/amsokol/openshift-golang-template/example/cmd/server`.
GOPROJECT_ROOT is set to `github.com/amsokol/openshift-golang-template/example`.
So GOPROJECT_CMD is set to `cmd/server`
```

14. Leave other options with default values and click `Create` and wait while pod is created

## [Optional] How to add health (liveness and readiness) checks
1. Login to OpenShift console using browser (eg https://openshift.example.com:8443) with Developer account

2. Open `project1` project

3. Click `Applications -> Deployments`

4. Click `golang1` in the list

5. Select `Configuration` tab

6. Click `Add Health Checks`

7. Click `Add Readiness Probe`

8. Set `Path` to '/healthz/ready' and leave other options with default values

9. Click `Add Liveness Probe`

10. Set `Path` to '/healthz/live' and leave other options with default values

11. Click `Save` and wait while pod is created

## Helper #1 - you can try golang template using S2I:
```
s2i build https://github.com/amsokol/openshift-golang-template.git amsokol/golang-openshift:1.8.3-2 golang1 -e GOPROJECT_ROOT=github.com/amsokol/openshift-golang-template/example -e GOPROJECT_CMD=cmd/server --context-dir /example
```
