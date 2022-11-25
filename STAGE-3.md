# Webscale

<img height="50" src="images/multicloud.png" />  


Try going multicloud, first destroy our local applications
```pwsh
waypoint destroy
```

Update the original app to be deployed to Google Cloud Run
```pwsh
registry {
  use "docker" {
    image = "gcr.io/your-gcp-project-name/waypint"
    tag   = "dev"
  }
}

deploy {
  use "google-cloud-run" {
    project  = "your-gcp-project-name"
    location = "australia-southeast1"

    capacity {
      memory                     = 256
      cpu_count                  = 2
      max_requests_per_container = 10
      request_timeout            = 300
    }
  }
}

release {
  use "google-cloud-run" {}
}
```

Create a new app in the same project so we can just "waypoint up" again and deploy to AWS...  
(this won't be supported in future versions of waypoint but is convenient for this demo)
```pwsh
app "waypint-app-aws" {

  path = "./waypint"
  
  build {
    use "pack" {
      builder = "paketobuildpacks/builder:full"
    }
    registry {
        use "aws-ecr" {
        region = "ap-southeast-2"
        tag = "dev"
      }
    }
  }
  
  deploy {
    use "aws-ecs" {
      region = "ap-southeast-2"
      memory = 1024
    }
  }
}
```
> This ECS deploy will likely say "deploy is not ready" because the default target group has 30 second health check intervals which means the best-case scenario is 1:30 for the task to become healthy?  
[AWS recommendations for ECS load balancer health checks](https://docs.aws.amazon.com/AmazonECS/latest/bestpracticesguide/load-balancer-healthcheck.html)  
[Potential changes to waypoint](https://github.com/hashicorp/waypoint/compare/7dd1e69465ae286d5cb3658fa39718385cdcab1f...ShaunLawrie:waypoint-ecs-healthchecks:main)

### This was based on the documentation at:
https://developer.hashicorp.com/waypoint/tutorials/get-started-kubernetes/get-started-kubernetes.  

🪟🙏 I'm a bit of a Windows nerd and have something for vagrant over here to check out, thumb it up if you actually use hyper-v https://github.com/hashicorp/vagrant/issues/8384#issuecomment-1228509296
