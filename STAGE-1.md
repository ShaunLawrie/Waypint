# Docker

<img height="50" src="images/docker.png" />  


### 1. Create a new mvc web application for dotnet core
```pwsh
cd src
dotnet new mvc --name "waypint"
```

### 2. Validate that our site does something
```pwsh
cd waypint
dotnet run
```

### 3. Install waypoint on docker
```pwsh
waypoint install --platform="docker" -accept-tos
```

### 4. Check out our shiny new deployment UI 
```pwsh
waypoint ui
```

### 5. Setup our project for use with waypoint
```pwsh
cd ..
waypoint init
# say yes and follow the prompts choosing docker every time and no release option
# DO NOT CHOOSE LATEST AS THE TAG FOR YOUR APPLICATION WHILE DEVELOPING
```
[Another reason not to chose "latest" as an image tag](https://github.com/hashicorp/waypoint/blob/main/builtin/k8s/platform.go#L345)

### 6. Update the waypoint.hcl file:
To use the directory with our code, the big builder from paketo and local docker.
```pwsh
path = "./waypint"
builder = "paketobuildpacks/builder:full"
local = true
```

### 7. Build and deploy our application
```pwsh
waypoint up
```
While we're waiting, [what are cloud native buildpacks?](https://tanzu.vmware.com/developer/guides/cnb-what-is/)

### 8. Check out the hostname service
We can choose our own test domain...
```pwsh
waypoint hostname register -app="waypint" waypint
```
https://waypint.waypoint.run/

<hr />  

## [Next -> Kubernetes](STAGE-2.md)