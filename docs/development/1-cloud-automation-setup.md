# Cloud Automation Setup

The Cloud Automation steps are run from a dedicated Ubuntu host in order to minimize potential differences between host environments.

```sh
ssh ubuntu@<IP Address of host> 
```

The ACC-8940 role will be used to create the required resources on AWS under the "staging" profile. The following AWS config file is required at `~/.aws/config`:

```conf
[default]
output = json
region = us-west-2
credential_source = Ec2InstanceMetadata

[profile staging]
output = json
region = us-west-2
role_arn = arn:aws:iam::119548034047:role/ACC-8940
source_profile=staging
```

## Install Gen3 Resources on AWS

The following commands are an example of using Cloud Automation to set up a staging deployment using config files from an existing development deployment. The same pattern would apply for any other deployment given an existing setup (e.g. setting up a staging environment using an existing staging environment).

```sh
# Take on the staging profile identity
gen3 workon staging aced-commons-staging
gen3 cd

# Copy the existing config from the staging deployment
mv config.tfvars config.tfvars.bak
cp ../aced-commons-staging/config.tfvars .

# Update the following values to match the staging deployment
vim config.tfvars
```

```diff
8c8
< vpc_name = "aced-commons-staging"
---
> vpc_name = "aced-commons-development"
11c11
< vpc_cidr_block = "172.31.0.0/16"
---
> vpc_cidr_block = "172.32.0.0/16"
93c93
< hostname = "development.aced-idp.org"
---
> hostname = "staging.aced-idp.org"
```

Create and apply the new Terraform plan:

```sh
gen3 tfplan # ~1 minute

gen3 tfapply # ~10 minutes

# Backup the Terraform output
cp -r aced-commons-staging_output/ ~/backups/
```

Make a note of the `<FENCE BOT USER SECRET>` in the output as this will be the only time the value is displayed:

```sh
# Outputs:
# aws_region = us-west-2
# data-bucket_name = aced-commons-staging-data-bucket
# fence-bot_user_id = <FENCE BOT USER ID>
# fence-bot_user_secret = <FENCE BOT USER SECRET>
```

## Install Kubernetes Cluster on AWS

```sh
# Take on the staging profile identity
gen3 workon staging aced-commons-staging_eks
gen3 cd

# Copy the existing config from the staging deployment
mv config.tfvars config.tfvars.bak
cp ../aced-commons-staging_eks/config.tfvars .

# Update the following values to match the staging deployment
vim config.tfvars
```

```diff
5c5
< vpc_name      = "aced-commons-development"
---
> vpc_name      = "aced-commons-staging"
7,8c7,8
< ec2_keyname   ="aced-commons-development_automation_dev"
< users_policy  = "aced-commons-development"
---
> ec2_keyname   ="aced-commons-staging_automation_dev"
> users_policy  = "aced-commons-staging"
```

Create and apply the new Terraform plan:

```sh
gen3 tfplan # ~1 minute

gen3 tfapply # ~10 minutes

# Backup the Terraform output
cp -r aced-commons-staging_output/ ~/backups/
```

## Resources

- <https://github.com/ACED-IDP/cloud-automation>
- <https://github.com/uc-cdis/cloud-automation>
- <https://github.com/uc-cdis/cloud-automation/blob/master/doc/csoc-free-commons-steps.md>
