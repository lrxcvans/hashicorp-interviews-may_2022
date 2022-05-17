# Getting Started with Terraform

Terraform is an open-source Infrastruture as Code (IaC) tool that enables organizations to automate the management and deployment of infrastructures and services. 

This document will introduce a new user on the use of Terraform, to include running actual Terraform commands that will automate the deploymnt of resources.

## Prequisites

Running the lab requires setting up a working environment to install and run Terraform commands.

To install Terraform, visit [Terraform.io](https://www.terraform.io/downloads.html) and download the appropriate zip archive for your system.

The system where Terraformm will be installed requires internet access to download and run the Terraform commands.

## Setting up your Terraform Environment

With Terraform installed, you can now start creating some infrastructure.

To run Terraform create a new directory on your local machine and add Terraform configuration code inside it.

Issue the commands below to create a terraform-demo directory on your system. The directory can be deleted after you complete the demo.

```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Next, create a file for your Terraform configuration code.

```shell
$ touch main.tf
```

Paste the following lines into the file.

```hcl
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
    }
  }
}
provider "docker" {
    host = "unix:///var/run/docker.sock"
}
resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

## Running Terraform

Initialize Terraform with the `init` command. The AWS provider will be installed. 

```shell
$ terraform init
```

You shoud check for any errors. If it ran successfully, provision the resource with the `apply` command.

```shell
$ terraform apply
```

The command will take up to a few minutes to run and will display a message indicating that the resource was created.

Finally, destroy the infrastructure.

```shell
$ terraform destroy
```

Look for a message are the bottom of the output asking for confirmation. Type `yes` and hit ENTER. Terraform will destroy the resources it had created earlier.

## Next Steps
