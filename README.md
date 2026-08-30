# Azure Artifacts Feed – Universal Packages Practice Lab

## Project Title

**Azure Artifacts Universal Package Management using Azure DevOps and macOS**

---

# 1. Objective

In this practice, we will:

* Create an Azure DevOps project
* Create an Azure Artifacts Feed
* Configure feed permissions
* Create a basic application/package
* Create a Git repository
* Install Azure CLI on macOS
* Install Azure DevOps CLI extension
* Authenticate Azure CLI
* Authenticate Azure DevOps using PAT
* Publish a Universal Package
* Verify package in Azure Artifacts
* Download the package
* Publish multiple package versions
* Download the latest package
* Automate package publishing using Azure Pipeline
* Automate package downloading using Azure Pipeline
* Understand common Azure Artifacts errors

---

# 2. Architecture

```text
Developer
   |
   | macOS Terminal
   |
   +----------------------+
   |                      |
   v                      v
Git Repository       Azure CLI
   |                      |
   | git push             | az artifacts
   v                      v
Azure Repos/GitHub   Azure DevOps
                          |
                          v
                   Azure Artifacts
                          |
                          v
                       Feed
                          |
                          v
                  Universal Package
                          |
             +------------+------------+
             |                         |
             v                         v
         Publish                    Download
```

---

# 3. Technologies Used

```text
macOS
Git
GitHub / Azure Repos
Azure CLI
Azure DevOps CLI Extension
Azure DevOps
Azure Artifacts
Universal Packages
Azure Pipelines
YAML
Python
Shell Script
JSON
```

---

# 4. Example Environment

For practice:

```text
Azure DevOps Organization:
cloudnautic

Azure DevOps Project:
project

Organization URL:
https://dev.azure.com/cloudnautic

Project URL:
https://dev.azure.com/cloudnautic/project

Feed:
newfeed

Package:
cloudnautic-app

Version:
1.0.0

Repository:
AzureArtifacts
```

You can replace these values with your own organization, project, feed and package names.

---

# 5. Azure DevOps Organization

Open Azure DevOps.

```text
https://dev.azure.com/
```

Create or select your organization.

Example:

```text
Organization:

cloudnautic
```

Organization URL:

```text
https://dev.azure.com/cloudnautic
```

---

# 6. Create Azure DevOps Project

Azure DevOps:

```text
Organization
    ↓
New Project
```

Configure:

```text
Project Name:
project

Visibility:
Private

Version Control:
Git

Work Item Process:
Basic
```

Click:

```text
Create
```

Project URL:

```text
https://dev.azure.com/cloudnautic/project
```

---

# 7. Enable Azure Artifacts

Inside project:

```text
Azure DevOps
    ↓
project
    ↓
Artifacts
```

Azure Artifacts is used as a centralized package management service.

It can store packages such as:

```text
Universal Packages
NuGet
npm
Maven
Python
Cargo
```

For this lab we will use:

```text
Universal Packages
```

Universal Packages are useful when your package is not tied to one specific package ecosystem.

For example:

```text
Configuration files
Shell scripts
Python scripts
Compiled applications
Deployment scripts
Terraform modules
Templates
ZIP content
Application bundles
```

---

# 8. Create Azure Artifacts Feed

Navigate:

```text
Azure DevOps
    ↓
project
    ↓
Artifacts
    ↓
Create Feed
```

Configure:

```text
Name:
newfeed
```

Visibility:

```text
People in my organization
```

Upstream Sources:

```text
Optional
```

Scope:

```text
Project: project
```

Recommended for this lab:

```text
Project-scoped feed
```

Click:

```text
Create
```

Azure DevOps supports both project-scoped and organization-scoped feeds.

---

# 9. Feed Permission Settings

Open:

```text
Artifacts
    ↓
newfeed
    ↓
Feed Settings
    ↓
Permissions
```

Common roles include:

```text
Feed Reader

Feed and Upstream Reader
/ Collaborator

Feed Publisher
/ Contributor

Feed Owner
```

For manually publishing using your own Azure DevOps account, make sure your account has permission to publish.

Recommended:

```text
Your User
    ↓
Feed Publisher (Contributor)
```

For pipeline publishing, configure the Build Service identity.

Example:

```text
project Build Service (cloudnautic)
```

Permission:

```text
Feed Publisher (Contributor)
```

For pipelines that only download:

```text
Feed and Upstream Reader
```

Microsoft specifically notes that pipeline build identities need feed permissions; publishing requires an appropriate publisher/contributor role.

---

# 10. Create Git Repository

You can use:

```text
Azure Repos

OR

GitHub
```

Example GitHub repository:

```text
AzureArtifacts
```

Clone:

```bash
git clone https://github.com/<username>/AzureArtifacts.git
```

Example:

```bash
git clone https://github.com/atulkamble/AzureArtifacts.git
```

Move inside repository:

```bash
cd AzureArtifacts
```

---

# 11. Create Repository Structure

Create directories:

```bash
mkdir package
mkdir pipelines
mkdir downloads
```

Create README:

```bash
touch README.md
```

Move into package:

```bash
cd package
```

Create files:

```bash
touch app.py
touch config.json
touch deploy.sh
```

Return:

```bash
cd ..
```

Repository:

```text
AzureArtifacts/
│
├── package/
│   ├── app.py
│   ├── config.json
│   └── deploy.sh
│
├── pipelines/
│   ├── publish-package.yml
│   └── download-package.yml
│
├── downloads/
│
├── .gitignore
│
└── README.md
```

---

# 12. Create Basic Python Application

Open:

```bash
nano package/app.py
```

Add:

```python
def hello() -> None:
    print("Hello from the Azure Artifacts package")


if __name__ == "__main__":
    hello()
```

Save:

```text
Control + O
Enter
Control + X
```

Test:

```bash
python3 package/app.py
```

Expected:

```text
Hello from the Azure Artifacts package
```

---

# 13. Create Configuration File

Open:

```bash
nano package/config.json
```

Add:

```json
{
  "application": "cloudnautic-app",
  "environment": "training",
  "version": "1.0.0"
}
```

---

# 14. Create Deployment Script

Open:

```bash
nano package/deploy.sh
```

Add:

```bash
#!/bin/bash

echo "Starting application deployment"
echo "Package version: 1.0.0"

python3 app.py

echo "Deployment completed"
```

Make executable:

```bash
chmod +x package/deploy.sh
```

Check:

```bash
ls -l package
```

---

# 15. Test Package Locally

Move:

```bash
cd package
```

Run:

```bash
./deploy.sh
```

Expected:

```text
Starting application deployment
Package version: 1.0.0
Hello from the Azure Artifacts package
Deployment completed
```

Return:

```bash
cd ..
```

---

# 16. Create .gitignore

```bash
touch .gitignore
```

Add:

```text
.DS_Store
downloads/*
!downloads/.gitkeep
```

Optional:

```bash
touch downloads/.gitkeep
```

---

# 17. Check Repository

Install tree if required:

```bash
brew install tree
```

Run:

```bash
tree
```

Expected:

```text
.
├── README.md
├── downloads
├── package
│   ├── app.py
│   ├── config.json
│   └── deploy.sh
└── pipelines
```

---

# 18. Push Code to Git

Check:

```bash
git status
```

Add:

```bash
git add .
```

Commit:

```bash
git commit -m "Add Azure Artifacts Universal Package practice"
```

Check branch:

```bash
git branch
```

Push:

```bash
git push origin main
```

---

# 19. Install Homebrew on Mac

Check:

```bash
brew --version
```

If Homebrew is already installed, continue.

---

# 20. Install Azure CLI on Mac

Install:

```bash
brew update
```

```bash
brew install azure-cli
```

Check:

```bash
az version
```

or:

```bash
az --version
```

---

# 21. Azure Login

Run:

```bash
az login
```

Browser authentication opens.

Login using the Microsoft account associated with Azure DevOps.

Check:

```bash
az account show
```

List subscriptions:

```bash
az account list -o table
```

Azure Artifacts itself is an Azure DevOps service, so the Azure subscription is not the important part of this lab. The important authentication target is your Azure DevOps organization.

---

# 22. Install Azure DevOps CLI Extension

Install:

```bash
az extension add --name azure-devops
```

Check extensions:

```bash
az extension list -o table
```

Update extension:

```bash
az extension update --name azure-devops
```

Microsoft requires Azure CLI plus the Azure DevOps extension for these Universal Package CLI commands.

---

# 23. Check Azure DevOps CLI

```bash
az devops -h
```

Check version:

```bash
az version
```

---

# 24. Configure Azure DevOps Defaults

Set organization:

```bash
az devops configure --defaults organization=https://dev.azure.com/cloudnautic
```

Set organization and project:

```bash
az devops configure --defaults \
  organization=https://dev.azure.com/cloudnautic \
  project=project
```

Check:

```bash
az devops configure --list
```

Expected:

```text
organization = https://dev.azure.com/cloudnautic
project = project
```

Microsoft's Universal Package setup documentation recommends configuring the organization and project defaults this way.

---

# 25. Azure DevOps Authentication Methods

You have two common options.

## Method 1 – Microsoft Account

```bash
az login
```

Then:

```bash
az devops configure --defaults \
  organization=https://dev.azure.com/cloudnautic \
  project=project
```

Try:

```bash
az devops project show --project project
```

---

# 26. Method 2 – Personal Access Token

PAT authentication is useful when normal account authentication is unsuccessful or when you receive authorization errors.

Create PAT:

```text
Azure DevOps
    ↓
User Settings
    ↓
Personal Access Tokens
    ↓
New Token
```

Configure:

```text
Name:
azure-artifacts-token

Organization:
cloudnautic

Expiration:
Choose required duration
```

Permissions:

```text
Packaging
    ↓
Read & write
```

Create token.

Copy the PAT immediately.

Microsoft recommends `Packaging > Read & write` for Universal Package PAT authentication.

---

# 27. Login to Azure DevOps Using PAT

Run:

```bash
az devops login --organization https://dev.azure.com/cloudnautic
```

Prompt:

```text
Token:
```

Paste PAT.

On macOS Terminal, the token may not visually appear while you paste it.

Press:

```text
Enter
```

---

# 28. Better PAT Authentication on Mac

Instead of typing the PAT directly into commands, use:

```bash
read -s AZURE_DEVOPS_PAT
```

Paste PAT.

Press:

```text
Enter
```

Export:

```bash
export AZURE_DEVOPS_EXT_PAT="$AZURE_DEVOPS_PAT"
```

Test:

```bash
az devops project show \
  --organization https://dev.azure.com/cloudnautic \
  --project project
```

Do **not** put your PAT inside Git repositories.

Bad:

```bash
TOKEN="abcdefghijklmnop..."
```

Never commit tokens.

---

# 29. Verify Project

```bash
az devops project show \
  --organization https://dev.azure.com/cloudnautic \
  --project project \
  -o table
```

---

# 30. List Azure DevOps Projects

```bash
az devops project list \
  --organization https://dev.azure.com/cloudnautic \
  -o table
```

---

# 31. List Feeds

Run:

```bash
az artifacts feed list \
  --organization https://dev.azure.com/cloudnautic \
  --project project \
  -o table
```

If successful, you should see:

```text
newfeed
```

---

# 32. Environment Variables

Instead of repeatedly typing values:

```bash
export ORGANIZATION="https://dev.azure.com/cloudnautic"
export PROJECT="project"
export FEED="newfeed"
export PACKAGE_NAME="cloudnautic-app"
export PACKAGE_VERSION="1.0.0"
export PACKAGE_PATH="./package"
```

Verify:

```bash
echo $ORGANIZATION
echo $PROJECT
echo $FEED
echo $PACKAGE_NAME
echo $PACKAGE_VERSION
echo $PACKAGE_PATH
```

---

# 33. Publish Universal Package

Project-scoped feed:

```bash
az artifacts universal publish \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "$PACKAGE_VERSION" \
  --path "$PACKAGE_PATH" \
  --description "Cloudnautic training package version $PACKAGE_VERSION"
```

Microsoft's current documented syntax for a project-scoped Universal Package follows this structure.

---

# 34. Full Command Without Variables

```bash
az artifacts universal publish \
  --organization "https://dev.azure.com/cloudnautic" \
  --project "project" \
  --scope project \
  --feed "newfeed" \
  --name "cloudnautic-app" \
  --version "1.0.0" \
  --path "./package" \
  --description "Cloudnautic training package version 1.0.0"
```

---

# 35. Package Naming Rules

Recommended:

```text
cloudnautic-app
```

Other valid examples:

```text
web-app
python-app
deployment_package
my.package
```

Keep package names lowercase.

Universal Package names must start and end with an alphanumeric character and support lowercase letters, numbers, dashes, underscores and periods within Microsoft's naming restrictions.

---

# 36. Versioning

Use semantic versioning:

```text
MAJOR.MINOR.PATCH
```

Examples:

```text
1.0.0
1.0.1
1.1.0
2.0.0
```

Meaning:

```text
1.0.0
│ │ │
│ │ └── Patch
│ └──── Minor
└────── Major
```

---

# 37. Verify Package in Azure DevOps

Open:

```text
Azure DevOps
    ↓
project
    ↓
Artifacts
    ↓
newfeed
```

You should see:

```text
cloudnautic-app
```

Select package.

Version:

```text
1.0.0
```

Files:

```text
app.py
config.json
deploy.sh
```

---

# 38. Download Universal Package

Create destination:

```bash
mkdir -p downloads/v1
```

Download:

```bash
az artifacts universal download \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "$PACKAGE_VERSION" \
  --path "./downloads/v1"
```

This matches Microsoft's current CLI approach for downloading from a project-scoped feed.

---

# 39. Verify Downloaded Package

```bash
tree downloads/v1
```

Expected:

```text
downloads/v1
├── app.py
├── config.json
└── deploy.sh
```

Run:

```bash
cd downloads/v1
```

```bash
chmod +x deploy.sh
```

```bash
./deploy.sh
```

Return:

```bash
cd ../..
```

---

# 40. Publish Version 1.0.1

Modify:

```bash
nano package/app.py
```

Example:

```python
def hello() -> None:
    print("Hello from Azure Artifacts package version 1.0.1")


if __name__ == "__main__":
    hello()
```

Update config:

```bash
nano package/config.json
```

```json
{
  "application": "cloudnautic-app",
  "environment": "training",
  "version": "1.0.1"
}
```

Set:

```bash
export PACKAGE_VERSION="1.0.1"
```

Publish:

```bash
az artifacts universal publish \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "$PACKAGE_VERSION" \
  --path "$PACKAGE_PATH" \
  --description "Cloudnautic package version $PACKAGE_VERSION"
```

---

# 41. Important Version Rule

You cannot normally republish the same version after it already exists.

For example, after publishing:

```text
1.0.0
```

do not attempt to publish:

```text
1.0.0
```

again.

Use:

```text
1.0.1
```

then:

```text
1.0.2
```

then:

```text
1.1.0
```

---

# 42. Download Latest Package

Create directory:

```bash
mkdir -p downloads/latest
```

Download:

```bash
az artifacts universal download \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "*" \
  --path "./downloads/latest"
```

Microsoft supports wildcard versions such as:

```text
*
1.*
1.2.*
```

for downloading the latest matching version.

---

# 43. Download Latest 1.x Version

```bash
az artifacts universal download \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "1.*" \
  --path "./downloads/latest"
```

---

# 44. Download Latest 1.0.x Version

```bash
az artifacts universal download \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "1.0.*" \
  --path "./downloads/latest"
```

---

# 45. Git Commit Package Changes

```bash
git add .
```

```bash
git commit -m "Publish package version 1.0.1"
```

```bash
git push origin main
```

---

# 46. Azure Pipeline for Universal Package Publishing

Create:

```bash
nano azure-pipelines.yml
```

Add:

```yaml
trigger:
  - main

pool:
  vmImage: ubuntu-latest

variables:
  packageName: 'cloudnautic-app'
  packageVersion: '1.0.$(Build.BuildId)'
  feedName: 'newfeed'

steps:

- script: |
    echo "Azure Artifacts Universal Package"
    echo "Package: $(packageName)"
    echo "Version: $(packageVersion)"
    echo "Feed: $(feedName)"

    echo "Package contents:"
    ls -la package
  displayName: 'Check Package'

- task: UniversalPackages@0
  displayName: 'Publish Universal Package'
  inputs:
    command: 'publish'
    publishDirectory: '$(Build.SourcesDirectory)/package'
    feedsToUsePublish: 'internal'
    vstsFeedPublish: '$(feedName)'
    vstsFeedPackagePublish: '$(packageName)'
    versionOption: 'custom'
    versionPublish: '$(packageVersion)'
    packagePublishDescription: 'Universal package published from Azure Pipeline'
```

---

# 47. Pipeline Version Example

If:

```text
Build.BuildId = 25
```

Package becomes:

```text
cloudnautic-app
```

Version:

```text
1.0.25
```

Next build:

```text
1.0.26
```

This prevents duplicate version errors.

---

# 48. Pipeline Feed Permissions

Before running pipeline:

```text
Artifacts
    ↓
newfeed
    ↓
Feed Settings
    ↓
Permissions
```

Add:

```text
project Build Service (cloudnautic)
```

Role:

```text
Feed Publisher (Contributor)
```

Depending on your Azure DevOps organization configuration, you may also encounter:

```text
Project Collection Build Service (cloudnautic)
```

For pipeline publishing, ensure the relevant build service identity has publisher access.

---

# 49. Commit Pipeline

```bash
git add azure-pipelines.yml
```

```bash
git commit -m "Add Azure Artifacts publish pipeline"
```

```bash
git push origin main
```

---

# 50. Create Azure Pipeline

Azure DevOps:

```text
Pipelines
    ↓
New Pipeline
```

Select source:

```text
GitHub

OR

Azure Repos Git
```

Select repository:

```text
AzureArtifacts
```

Choose:

```text
Existing Azure Pipelines YAML file
```

Path:

```text
/azure-pipelines.yml
```

Click:

```text
Run
```

---

# 51. Pipeline Flow

```text
Git Push
   |
   v
Azure Pipeline
   |
   v
Read package/
   |
   v
UniversalPackages@0
   |
   v
Azure Artifacts
   |
   v
newfeed
   |
   v
cloudnautic-app
   |
   v
1.0.BuildID
```

---

# 52. Separate Download Pipeline

Create:

```bash
nano pipelines/download-package.yml
```

Add:

```yaml
trigger: none

pool:
  vmImage: ubuntu-latest

steps:

- task: UniversalPackages@0
  displayName: 'Download Universal Package'
  inputs:
    command: 'download'
    feedsToUse: 'internal'
    vstsFeed: 'project/newfeed'
    vstsFeedPackage: 'cloudnautic-app'
    vstsPackageVersion: '1.0.1'
    downloadDirectory: '$(Build.SourcesDirectory)/downloaded-package'

- script: |
    echo "Downloaded package:"
    ls -la $(Build.SourcesDirectory)/downloaded-package

    echo "Package files:"
    find $(Build.SourcesDirectory)/downloaded-package -type f
  displayName: 'Verify Package'
```

Microsoft's documented pipeline task for Universal Package downloading is `UniversalPackages@0`.

---

# 53. Download Latest Package in Pipeline

You can change:

```yaml
vstsPackageVersion: '1.0.1'
```

to:

```yaml
vstsPackageVersion: '*'
```

when supported by the desired download workflow.

---

# 54. Recommended Final Repository Structure

```text
AzureArtifacts/
│
├── package/
│   ├── app.py
│   ├── config.json
│   └── deploy.sh
│
├── pipelines/
│   ├── publish-package.yml
│   └── download-package.yml
│
├── downloads/
│   └── .gitkeep
│
├── .gitignore
│
├── azure-pipelines.yml
│
└── README.md
```

---

# 55. Useful CLI Commands

Azure login:

```bash
az login
```

Azure account:

```bash
az account show
```

Azure DevOps extension:

```bash
az extension add --name azure-devops
```

Update:

```bash
az extension update --name azure-devops
```

Configure:

```bash
az devops configure --defaults \
  organization=https://dev.azure.com/cloudnautic \
  project=project
```

Show configuration:

```bash
az devops configure --list
```

PAT login:

```bash
az devops login \
  --organization https://dev.azure.com/cloudnautic
```

Logout:

```bash
az devops logout
```

List projects:

```bash
az devops project list \
  --organization https://dev.azure.com/cloudnautic \
  -o table
```

List feeds:

```bash
az artifacts feed list \
  --organization https://dev.azure.com/cloudnautic \
  --project project \
  -o table
```

---

# 56. Complete Mac Practice Command Sequence

Run these commands in sequence.

```bash
# Clone repository
git clone https://github.com/<username>/AzureArtifacts.git

cd AzureArtifacts

# Create structure
mkdir -p package
mkdir -p pipelines
mkdir -p downloads

touch README.md
touch .gitignore

touch package/app.py
touch package/config.json
touch package/deploy.sh

# Make deployment script executable
chmod +x package/deploy.sh

# Install Azure CLI
brew update
brew install azure-cli

# Check Azure CLI
az version

# Azure login
az login

# Install Azure DevOps extension
az extension add --name azure-devops

# Update extension
az extension update --name azure-devops

# Configure defaults
az devops configure --defaults \
  organization=https://dev.azure.com/cloudnautic \
  project=project

# Check defaults
az devops configure --list

# Authenticate Azure DevOps using PAT if required
az devops login \
  --organization https://dev.azure.com/cloudnautic

# Test project
az devops project show \
  --organization https://dev.azure.com/cloudnautic \
  --project project

# List feeds
az artifacts feed list \
  --organization https://dev.azure.com/cloudnautic \
  --project project \
  -o table

# Variables
export ORGANIZATION="https://dev.azure.com/cloudnautic"
export PROJECT="project"
export FEED="newfeed"
export PACKAGE_NAME="cloudnautic-app"
export PACKAGE_VERSION="1.0.0"
export PACKAGE_PATH="./package"

# Verify variables
echo $ORGANIZATION
echo $PROJECT
echo $FEED
echo $PACKAGE_NAME
echo $PACKAGE_VERSION
echo $PACKAGE_PATH

# Publish
az artifacts universal publish \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "$PACKAGE_VERSION" \
  --path "$PACKAGE_PATH" \
  --description "Cloudnautic training package $PACKAGE_VERSION"

# Create download location
mkdir -p downloads/v1

# Download
az artifacts universal download \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "$PACKAGE_VERSION" \
  --path "./downloads/v1"

# Check downloaded package
tree downloads/v1
```

---

# 57. Publish New Version

```bash
export PACKAGE_VERSION="1.0.1"
```

```bash
az artifacts universal publish \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "$PACKAGE_VERSION" \
  --path "$PACKAGE_PATH" \
  --description "Cloudnautic training package $PACKAGE_VERSION"
```

---

# 58. Download Latest

```bash
mkdir -p downloads/latest
```

```bash
az artifacts universal download \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "*" \
  --path "./downloads/latest"
```

---

# 59. Common Error – TF400813

Error:

```text
TF400813:
The user is not authorized to access this resource.
```

Example:

```text
Microsoft.VisualStudio.Services.Common.VssServiceException:
TF400813: The user is not authorized to access this resource.
```

Possible causes:

```text
Wrong Azure DevOps account

PAT belongs to another account

PAT does not have Packaging permissions

User does not have access to project

User does not have permission on feed

Wrong Azure DevOps organization

Expired PAT

Stale cached Azure DevOps authentication
```

---

# 60. Fix TF400813

Logout:

```bash
az devops logout
```

Check:

```bash
az account show
```

Login again:

```bash
az login
```

Configure:

```bash
az devops configure --defaults \
  organization=https://dev.azure.com/cloudnautic \
  project=project
```

Create new PAT with:

```text
Packaging
    ↓
Read & write
```

Login:

```bash
az devops login \
  --organization https://dev.azure.com/cloudnautic
```

Then test:

```bash
az devops project show \
  --organization https://dev.azure.com/cloudnautic \
  --project project
```

Then:

```bash
az artifacts feed list \
  --organization https://dev.azure.com/cloudnautic \
  --project project \
  -o table
```

Only after these work should you retry package publishing.

---

# 61. Debug Authentication

Check current Azure identity:

```bash
az account show \
  --query user \
  -o json
```

Check Azure DevOps:

```bash
az devops configure --list
```

Test organization/project:

```bash
az devops project show \
  --organization "$ORGANIZATION" \
  --project "$PROJECT"
```

Test feed:

```bash
az artifacts feed list \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  -o table
```

Then publish.

---

# 62. Common Error – Feed Not Found

Possible error:

```text
Feed not found
```

Check:

```bash
az artifacts feed list \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  -o table
```

Check that:

```text
FEED=newfeed
```

matches the actual feed exactly.

---

# 63. Common Error – Project Scope

If your feed is:

```text
Project scoped
```

you must use:

```bash
--project "$PROJECT"
--scope project
```

Example:

```bash
az artifacts universal publish \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "$PACKAGE_VERSION" \
  --path "$PACKAGE_PATH"
```

---

# 64. Organization-Scoped Feed

If you deliberately created an organization-scoped feed, the syntax is different.

Example:

```bash
az artifacts universal publish \
  --organization "$ORGANIZATION" \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "$PACKAGE_VERSION" \
  --path "$PACKAGE_PATH"
```

No:

```text
--scope project
```

is required for the organization-scoped example documented by Microsoft.

---

# 65. Common Error – Package Already Exists

If:

```text
cloudnautic-app 1.0.0
```

already exists, change:

```bash
export PACKAGE_VERSION="1.0.1"
```

Then publish again.

---

# 66. Common Error – Invalid Package Name

Avoid:

```text
Cloudnautic App
My Application
Azure Package
```

Use:

```text
cloudnautic-app
my-application
azure-package
```

---

# 67. Common macOS zsh Error

Wrong:

```bash
az artifacts universal publish \ 
```

There must be **nothing after the backslash**.

Correct:

```bash
az artifacts universal publish \
  --organization "$ORGANIZATION" \
  --project "$PROJECT"
```

The backslash means:

```text
continue command on next line
```

A space after `\` can cause unexpected zsh behavior.

---

# 68. Check Variables Before Publishing

Always run:

```bash
echo "Organization = $ORGANIZATION"
echo "Project      = $PROJECT"
echo "Feed         = $FEED"
echo "Package      = $PACKAGE_NAME"
echo "Version      = $PACKAGE_VERSION"
echo "Path         = $PACKAGE_PATH"
```

Check path:

```bash
ls -la "$PACKAGE_PATH"
```

---

# 69. Security Best Practices

Never place PAT inside:

```text
README.md
azure-pipelines.yml
deploy.sh
config.json
Git repository
GitHub commits
Screenshots
Training notes shared publicly
```

Use:

```bash
az devops login
```

or:

```bash
export AZURE_DEVOPS_EXT_PAT="..."
```

for temporary local authentication.

After practice:

```bash
unset AZURE_DEVOPS_PAT
```

```bash
unset AZURE_DEVOPS_EXT_PAT
```

---

# 70. Complete Practice Workflow

```text
1. Create Azure DevOps Organization

2. Create Project

3. Open Artifacts

4. Create Feed

5. Select Project Scope

6. Configure Feed Permissions

7. Create Git Repository

8. Clone Repository on Mac

9. Create package/

10. Create:
    app.py
    config.json
    deploy.sh

11. Test Application

12. Install Azure CLI

13. Install azure-devops extension

14. az login

15. Configure Azure DevOps organization/project

16. Create PAT if required

17. Packaging → Read & write

18. az devops login

19. Verify Project

20. Verify Feed

21. Configure Shell Variables

22. Publish Universal Package 1.0.0

23. Verify Package in Azure Artifacts

24. Download 1.0.0

25. Test Downloaded Package

26. Modify Application

27. Publish 1.0.1

28. Download Latest

29. Add Azure Pipeline

30. Give Build Service Feed Publisher permission

31. Run Pipeline

32. Verify pipeline-generated package version
```

---

# 71. Final End-to-End Flow

```text
Developer modifies application
             |
             v
        Git Commit
             |
             v
          Git Push
             |
             v
       Azure Pipeline
             |
             v
     UniversalPackages@0
             |
             v
      Azure Artifacts
             |
             v
          newfeed
             |
             v
     cloudnautic-app
             |
       +-----+-----+
       |     |     |
       v     v     v
     1.0.0 1.0.1 1.0.x
             |
             v
      Consumer Pipeline
             |
             v
          Download
             |
             v
           Deploy
```

---

# 72. Important Commands to Remember

```bash
az login
```

```bash
az extension add --name azure-devops
```

```bash
az extension update --name azure-devops
```

```bash
az devops configure --defaults \
  organization=https://dev.azure.com/cloudnautic \
  project=project
```

```bash
az devops login \
  --organization https://dev.azure.com/cloudnautic
```

Publish:

```bash
az artifacts universal publish \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "$PACKAGE_VERSION" \
  --path "$PACKAGE_PATH"
```

Download:

```bash
az artifacts universal download \
  --organization "$ORGANIZATION" \
  --project "$PROJECT" \
  --scope project \
  --feed "$FEED" \
  --name "$PACKAGE_NAME" \
  --version "$PACKAGE_VERSION" \
  --path "./downloads"
```

---

# 73. Practice Versions

For training, publish:

```text
cloudnautic-app
│
├── 1.0.0
├── 1.0.1
├── 1.0.2
├── 1.1.0
└── 2.0.0
```

This gives students practice with package lifecycle and semantic versioning.

---

# 74. Suggested Repository Name

```text
azure-artifacts-universal-packages
```

Alternative:

```text
azure-artifacts-practice
```

Recommended:

```text
azure-artifacts-universal-packages
```

---

# 75. Resume Project Description

**Azure Artifacts Universal Package Management**

Implemented an end-to-end package management solution using Azure DevOps and Azure Artifacts. Created project-scoped feeds, configured feed permissions, packaged application and deployment files as Universal Packages, implemented semantic versioning, published and downloaded packages through Azure CLI on macOS, and automated package publishing and consumption using Azure Pipelines and the UniversalPackages task.

---

# 76. Skills Covered

```text
Azure DevOps
Azure Artifacts
Universal Packages
Azure Pipelines
Azure CLI
Azure DevOps CLI
Git
GitHub / Azure Repos
PAT Authentication
Feed Permissions
Package Versioning
CI/CD
YAML
Python
Shell Scripting
macOS Terminal
```

---

# Final Result

After completing the lab:

```text
Source Code
    ↓
Git Repository
    ↓
Azure Pipeline
    ↓
Universal Package
    ↓
Azure Artifacts Feed
    ↓
Versioned Package Storage
    ↓
Download / Consume
    ↓
Deployment
```

This gives a complete basic Azure Artifacts workflow suitable for hands-on practice, classroom demonstration, GitHub documentation, and a beginner-level DevOps portfolio project.
