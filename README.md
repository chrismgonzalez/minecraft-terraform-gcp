# Minecraft Server - Terraform plan for GCP

This is a Terraform infrastructure provisioning template for deploying the resources to build your very own Minecraft server. By using a GCS bucket, game data is preserved across sessions.  The server is hosted on a permenant, static IP address so you only need to configure your client device and you're done!

In order to save costs, the user needs to start the VM each session, but it will shutdown within 24 hours if you forget to turn it off.

Features:

- Runs [itzg/minecraft-server](https://hub.docker.com/r/itzg/minecraft-server/) Docker image
- Preemtible VM (cheapest), shuts down automatically within 24h if you forget to stop the VM
- Reserves a stable public IP, so the minecraft clients do not need to be reconfigured
- Reserves the disk, so game data is remembered across sessions
- Restricted service account, VM has no ability to consume GCP resources beyond its instance and disk
- 2$ per month
- Reserved IP address costs: $1.46 per month
- Reserved 10Gb disk costs: $0.40
- VM cost: $0.01 per hour, max session cost $0.24

## Setup

<hr>

### Pre-requisites

- You have the [Google Cloud SDK](https://cloud.google.com/sdk/docs/quickstart) installed locally
- You have [Terraform](https://www.terraform.io/downloads) installed locally
- Create a GCP account (if you don't have one already) and a new project.
- Enable Google Cloud Storage for the associated project and create a GCS storage bucket.
- Enable Compute Engine for the associated project.

After following the above steps, copy and paste the Terraform code from `main.tf` into your local version. Add your project specific information the following information into the `main.tf` file:

```tf
terraform {
    backend "gcs" {
    prefix = "minecraft/state"
    bucket = "terraform-gonzohouse" # the name of your GCS bucket
  }
}

locals {
  project = "gonzohouse-minecraft" # your project ID
  region  = "us-central1" # a region and zone that is close to you.
  zone    = "us-central1-a"
```
