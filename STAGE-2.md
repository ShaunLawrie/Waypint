# Blue Green Deployments with Kubernetes

<img height="50" src="images/kube.png" />  


### 1. Remove Waypoint from Docker
```pwsh
docker ps --format "{{json .}}" |
  ConvertFrom-Json |
  Where-Object { $_.Names -like "*waypoint*" } |
  Foreach-Object {
    Write-Warning "Removing waypoint: $($_.Names)"
    docker stop $_.ID
    docker rm $_.ID
  }
docker volume rm "waypoint-server"
docker builder prune
```

### 2. Enable Kubernetes in Docker Desktop and Install Waypoint into Kubernetes
![Docker Desktop Enable Kubernetes Image](images/dockerkube.png)
```pwsh
waypoint install --platform="kubernetes" -accept-tos
```

### 3. Update the Waypoint HCL for Kubernetes Deployment
```hcl
deploy {
  use "kubernetes" {
    probe_path = "/"
  }
}

release {
  use "kubernetes" {
    load_balancer = true
    port          = 3000
  }
}
```

### 4. Deploy the Site on Kubernetes
```pwsh
waypoint up
```
While waiting for this check out the buildbacks metadata labels on docker images:
```pwsh
(docker image inspect waypint:dev | ConvertFrom-Json).Config.Labels."io.paketo.stack.packages" |
  ConvertFrom-Json |
  Select-Object -Property name, version
```

### 5. Deploy the Site on Kubernetes
Check out the new "prod site" at our load balancer address http://localhost:3000/

### 6. Edit the Website Homepage View
Edit the cshtml in `Views/Home/Index.cshtml` so there is a visible change in our next deployment.

### 7. Build the new version of the website
```pwsh
waypoint build
```

### 8. Build the new version of the website
```pwsh
waypoint deploy -release="false"
```
Notice that the preview URL shows the changes but the "prod site" at our load balancer address http://localhost:3000/ doesn't

### 9. Yolo no testing release it to prod
```pwsh
waypoint release
```
Now the change is live at http://localhost:3000/

### 10. Oh no that was catastrophic, let's rollback
```pwsh
waypoint release -deployment="ENTER_PREVIOUS_DEPLOYMENT_NUMBER"
```
Now the version at http://localhost:3000/ has been rolled back, note that because of network connection reuse you may need to close and reopen your browser to see this happen quicker.

🙏🐳

<hr />  

## [Next -> Cloud](STAGE-3.md)