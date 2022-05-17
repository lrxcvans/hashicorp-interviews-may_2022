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

A successful output from the 'terraform init' command is shown below.

```hcl
tfuser@ubuntu-tfsvr:~/terraform-demo# terraform init

Initializing the backend...

Initializing provider plugins...
- Finding latest version of kreuzwerker/docker...
- Installing kreuzwerker/docker v2.16.0...
- Installed kreuzwerker/docker v2.16.0 (self-signed, key ID BD080C4571C6104C)

Partner and community providers are signed by their developers.
If you'd like to know more about provider signing, you can read about it here:
https://www.terraform.io/docs/cli/plugins/signing.html

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
tfuser@ubuntu-tfsvr:~/terraform-demo#
```

Check the output for any errors. 

If it ran successfully, provision the resource with the `apply` command. Enter 'yes' when prompted to provision the resource.

```shell
$ terraform apply
```

A successful output from the 'terraform apply' command is shown below.

```hcl
tfuser@ubuntu-tfsvr:~/terraform-demo# terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the
following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach           = false
      + bridge           = (known after apply)
      + command          = (known after apply)
      + container_logs   = (known after apply)
      + entrypoint       = (known after apply)
      + env              = (known after apply)
      + exit_code        = (known after apply)
      + gateway          = (known after apply)
      + hostname         = (known after apply)
      + id               = (known after apply)
      + image            = (known after apply)
      + init             = (known after apply)
      + ip_address       = (known after apply)
      + ip_prefix_length = (known after apply)
      + ipc_mode         = (known after apply)
      + log_driver       = (known after apply)
      + logs             = false
      + must_run         = true
      + name             = "training"
      + network_data     = (known after apply)
      + read_only        = false
      + remove_volumes   = true
      + restart          = "no"
      + rm               = false
      + security_opts    = (known after apply)
      + shm_size         = (known after apply)
      + start            = true
      + stdin_open       = false
      + tty              = false

      + healthcheck {
          + interval     = (known after apply)
          + retries      = (known after apply)
          + start_period = (known after apply)
          + test         = (known after apply)
          + timeout      = (known after apply)
        }

      + labels {
          + label = (known after apply)
          + value = (known after apply)
        }

      + ports {
          + external = 80
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id          = (known after apply)
      + latest      = (known after apply)
      + name        = "nginx:latest"
      + output      = (known after apply)
      + repo_digest = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

docker_image.nginx: Creating...
docker_image.nginx: Creation complete after 8s [id=sha256:7425d3a7c478efbeb75f0937060117343a9a510f72f5f7ad9f14b1501a36940cnginx:latest]
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 4s [id=42e42af3a284efcb32a496cd37e3d2725d4ac7bde72d0866d602a02b424efd03]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
tfuser@ubuntu-tfsvr:~/terraform-demo#
```
The command will take up to a few minutes to run and will display a message indicating that the resource was created.

If the output displays errors, visit [Terraform.io](https://learn.hashicorp.com/tutorials/terraform/troubleshooting-workflow) to troubleshoot.

Finally, destroy the infrastructure if no errors from the 'apply' command.

```shell
$ terraform destroy
```


```hcl
tfuser@ubuntu-tfsvr:~/terraform-demo# terraform destroy
docker_image.nginx: Refreshing state... [id=sha256:7425d3a7c478efbeb75f0937060117343a9a510f72f5f7ad9f14b1501a36940cnginx:latest]
docker_container.nginx: Refreshing state... [id=42e42af3a284efcb32a496cd37e3d2725d4ac7bde72d0866d602a02b424efd03]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the
following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.nginx will be destroyed
  - resource "docker_container" "nginx" {
      - attach            = false -> null
      - command           = [
          - "nginx",
          - "-g",
          - "daemon off;",
        ] -> null
      - cpu_shares        = 0 -> null
      - dns               = [] -> null
      - dns_opts          = [] -> null
      - dns_search        = [] -> null
      - entrypoint        = [
          - "/docker-entrypoint.sh",
        ] -> null
      - env               = [] -> null
      - gateway           = "172.17.0.1" -> null
      - group_add         = [] -> null
      - hostname          = "42e42af3a284" -> null
      - id                = "42e42af3a284efcb32a496cd37e3d2725d4ac7bde72d0866d602a02b424efd03" -> null
      - image             = "sha256:7425d3a7c478efbeb75f0937060117343a9a510f72f5f7ad9f14b1501a36940c" -> null
      - init              = false -> null
      - ip_address        = "172.17.0.2" -> null
      - ip_prefix_length  = 16 -> null
      - ipc_mode          = "private" -> null
      - links             = [] -> null
      - log_driver        = "json-file" -> null
      - log_opts          = {} -> null
      - logs              = false -> null
      - max_retry_count   = 0 -> null
      - memory            = 0 -> null
      - memory_swap       = 0 -> null
      - must_run          = true -> null
      - name              = "training" -> null
      - network_data      = [
          - {
              - gateway                   = "172.17.0.1"
              - global_ipv6_address       = ""
              - global_ipv6_prefix_length = 0
              - ip_address                = "172.17.0.2"
              - ip_prefix_length          = 16
              - ipv6_gateway              = ""
              - network_name              = "bridge"
            },
        ] -> null
      - network_mode      = "default" -> null
      - privileged        = false -> null
      - publish_all_ports = false -> null
      - read_only         = false -> null
      - remove_volumes    = true -> null
      - restart           = "no" -> null
      - rm                = false -> null
      - security_opts     = [] -> null
      - shm_size          = 64 -> null
      - start             = true -> null
      - stdin_open        = false -> null
      - storage_opts      = {} -> null
      - sysctls           = {} -> null
      - tmpfs             = {} -> null
      - tty               = false -> null

      - ports {
          - external = 80 -> null
          - internal = 80 -> null
          - ip       = "0.0.0.0" -> null
          - protocol = "tcp" -> null
        }
    }

  # docker_image.nginx will be destroyed
  - resource "docker_image" "nginx" {
      - id          = "sha256:7425d3a7c478efbeb75f0937060117343a9a510f72f5f7ad9f14b1501a36940cnginx:latest" -> null
      - latest      = "sha256:7425d3a7c478efbeb75f0937060117343a9a510f72f5f7ad9f14b1501a36940c" -> null
      - name        = "nginx:latest" -> null
      - repo_digest = "nginx@sha256:2c72b42c3679c1c819d46296c4e79e69b2616fa28bea92e61d358980e18c9751" -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

docker_container.nginx: Destroying... [id=42e42af3a284efcb32a496cd37e3d2725d4ac7bde72d0866d602a02b424efd03]
docker_container.nginx: Destruction complete after 1s
docker_image.nginx: Destroying... [id=sha256:7425d3a7c478efbeb75f0937060117343a9a510f72f5f7ad9f14b1501a36940cnginx:latest]
docker_image.nginx: Destruction complete after 0s

Destroy complete! Resources: 2 destroyed.
tfuser@ubuntu-tfsvr:~/terraform-demo#
```

Look for a message at the bottom of the output asking for confirmation. Type `yes` and hit ENTER. Terraform will destroy the resources it had created earlier.

## Next Steps

You should have gained a basic understanding of how Terraform's Infrastructure as Code (IaC) tool can automate the management and deployment of infrastructures and services. 

To continue your journey with Terraform, we provide and extensive library of documentation on how Terraform can be used in a many types of environments. 

Visit out online tutorials [here](https://learn.hashicorp.com/) to find topics specific to your environment. 
