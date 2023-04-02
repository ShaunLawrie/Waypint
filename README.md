> **Warning**  
> These were my "slides" for an old presentation at the Hashicorp Meetup in Wellington.  
> They will be out of date for Waypoint by the time you read this.

# Waypint 🍺

This is my speed guide for deploying a dotnet core application with Hashicorp waypoint.  

I'll be progressing from deploying to Docker Desktop for local development to:
 - Deploying staged releases in Kubernetes on Docker Desktop for a more flexible local development setup.
 - AWS ECS to simulate deploying to a shared QA environment.
 - Google Cloud Run & AWS ECS to show the portability and simplicity afforded by using the opinionated Waypoint rather than hand crafting Yaml.

<p align="center">
  <img src="https://user-images.githubusercontent.com/13159458/203877303-06a91c2d-3791-4f1e-961a-7fdf3e7bdfad.png" />
</p>

# Intro
👋 I'm Shaun Lawrie from PartsTrader where I'm the Platform Engineering Lead.  

📈 [PartsTrader](https://www.partstrader.tech/) is a company that provides an online marketplace where repairers can efficiently provision parts for vehicle collision repairs.  

☁️ I'm currently using a mixture of tooling for AWS, Azure and on-prem tied together with:  
   GitHub Actions, Terraform, Pulumi, Lambdas, Octopus Deploy...   

🐋 This demo will be a scary live demo using Docker Desktop/Kubernetes and Waypoint over an unreliable internet connection...   

<hr />  

## [Next -> Docker](STAGE-1.md)
