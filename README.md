# Terraform Beginner Bootcamp 2023

#Semantic Versioning ! :mage:

This project is going to utilize semantic versioning for its tagging.
[semver.org](http://semver.org/)

The general format:
 **MAJOR.MINOR.PATCH**, eg. '1.0.1'

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

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
###Shebang

A Shebang (pronounced Sha-bang) tells the bash script what program that will interpet the script. eg. '#!/bin/bash'

ChatGPT recommended this format for bash: '#!/usr/bin/env bash'



- for portability 
- will search the user's Path for the bash executable

When executing bash script we can use the './'shorthand notiation to execute the bash script

[Shebang #!](https://en.wikipedia.org/wiki/Shebang_(Unix))

### Linux consideration

[Chmod - access rights in linux](https://en.wikipedia.org/wiki/Chmod)
[Gitpod documentation - (Check for code traps!)](https://www.gitpod.io/docs/configure/workspaces/workspace-lifecycle)

