# Terraform Beginner Bootcamp 2023

#Semantic Versioning ! :mage:

This project is going to utilize semantic versioning for its tagging.
[semver.org](http://semver.org/)

The general format:
 **MAJOR.MINOR.PATCH**, eg. '1.0.1'

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

 1_semantic_versioning
=======
## References Terraform CLI refactor

### Considerations with the Terraform CLI changes
The Terraform CLI installation instructions have changed due to gpg keyring changes. So we needed to refer to the latest install instructions via Terraform documentation and change the sripting for install.

[Install Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Considerations for Linux Distribution

This project is built against Ubunto. Please consider checking you Linux Distribution and change accordingly to your distribution needs.

[How to check os version in linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)


### Refactoring into Bash Scripts

While fixing the Terraform CLI gpg  deprecation issues, we noticed that bash script steps were a considerable amount more code so we decided to to create a bash script to install the Terraform CLI.

This bash script is located here: [./bin/install_terraform_CLI](./bin/install_terraform_CLI)

-This will keep the Gitpod Task File ([.gitpod.yml](.gitpod.yml) )tidy.
-This allows us an easier to debug and execute manually Terraform CLI install
-This will allow better portablility for other projects that need to install Terraform CLI.

[Check os version in linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

Example of checking OS Version
```sh
$ cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```
#### Shebang

A Shebang (pronounced Sha-bang) tells the bash script what program that will interpet the script. eg. '#!/bin/bash'

ChatGPT recommended this format for bash: '#!/usr/bin/env bash'

- for portability 
- will search the user's Path for the bash executable


[Shebang #! Wikipedia page](https://en.wikipedia.org/wiki/Shebang_(Unix))

### Execution considerations

When executing bash script we can use the './'shorthand notiation to execute the bash script.

eg. './bin/install_terraform_CLI'

When executing the bash script in .gitpod.yml we need to point the script to a program to interpertate it.

eg. 'source ./bin/install_terraform_CLI'

### Linux permissions consideration

In order to make our bash scripts executable we need to change linux permission for the fix to be executable at the user mode.

```sh
Chmod u+x ./bin/install_terraform_CLI
```
Alternatively;
```sh
chmod 744 ./bin/install_terraform_CLI
```

[Chmod - access rights in linux](https://en.wikipedia.org/wiki/Chmod)

### Github Lifecycle (Before, Init, Command)

We need to be careful when using the Init because it will not rerun if we restart an exiisting workspace.

[Gitpod documentation - (Check for code traps!)](https://www.gitpod.io/docs/configure/workspaces/workspace-lifecycle)

### Working Env Vars

#### env command

We van list out all Environment Variables (env Vars) using the 'env'command.

We can filter specific env vars using grep eg. 'env | grep AWS_'

#### Setting and unsetting Env Vars

In the terminal we van set using 'export Hello='World'

In the terminal we unset using 'unset Hello'
> Note to oneself, this would be a fun code trap. Check for weird variables.

We can set an env var temporarily when just running a command:

```sh
HELLO='world' ./bin/print_message
```

Within a bash script we van set env without writing export eg.

```sh
#!/usr/bin/env bash

HELLO='World'

echo $HELLO
```

## Printing Vars

We can print an env var using echo eg. 'echo $HELLO'

#### Scoping of Env Vars

When you open up new bash terminals in VSCode it will not be aware of env vars that you have set in another window.

If you want to Env Vars to persist across all future bash terminals that are open you need to set env vars in you bash profile (Global variables). eg. '.bash_profile'

#### Persisting Env Vars in Gitpod

We van persist env vars into gitpo by storing then in Gitpod Secret Storage.

```
gp env HELLO='World'
```

All future workspaces launched will s et the env vars for all bash terminals opened in those workspaces.

You can also set env vars in the '.gitpod.yml'but this can only contain non-sensitive env vars.

### AWS CLI Installation

AWS CLI is installed for the project via the bash script ['./bin/install_aws_cli'](./bin/install_aws_cli)


[Getting started Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
[AWS CLI env vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our AWS credentials is configured correctly by running the following AWS CLI command:
```sh
aws sts get-caller-identity
```

If it is succesfull you should see a json payload return that looks like this:

```json
{
    "UserId": "AIXXXXXXXXXXGZSO4TCFS",
    "Account": "910000000000",
    "Arn": "arn:aws:iam::910000000000:user/terraform-beginner-bootcamp-2023"
}
```

We'll need to generate AWS CLI credit from IAM User in order to the user AWS CLI.


Remember!!! when working with terraform, use registry.terraform.io

Providers = Like API with code to use to interact 
Modules = similar to templates to use in terraform.


## Terraform Basics

### Terraform Registry

Terraform sources their providers and modules from the Terraform registry which located at [registry.terraform.io](https://registry.terraform.io/)

- **Providers** is an interface to API's that will allow you to create resources in terraform.
- **Modules** are a way to refactor or to make large amount of terraform code modular, portable and sharable.


[Random Terraform Provider](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/string)

### Terraform Console

We can see a list of all the Terraform commands by simply typing 'terraform'

#### Terraform Init

'terraform init'

At the start of a new terraform project we will run 'terraform init'to download the binaries for the terraform providers that we will use in this project. 

#### Terraform Plan

'terraform plan'

This will generate out a changeset, about the state of our infrastructure and what will be changed.

We can output this changeset ie. "plan" to be passed to an apply, but often you van just ignore outputting.

#### Terraform Apply

'terraform apply'

This will run a plan and pass the changeset to be executed by terraform. Apply should prompt yes or no.

If we want to automatically approve and apply we van provide the 'terraform --auto-approve' flag.

## Terraform Lock Files

'.terraform.lock.hcl' contains the locked versioning for the providers or modules that should be used wih this project.

The Terraform Lock File **should be committed** to your Version Control System (VSC) eg. Github

### Terraform State Files

'.terraform.tfstate' contains information about the current state of your infrastructure.

This file **should not be committed** to your VCS.

This file can contain sensitive data.

If you lose this file, you lose knowing the state of your infrastructure. 

'.terraform.tfstate.backup' is the previous state file state..

### Terraform Directory

'.terraform' directory contains binaries of terraform providers.