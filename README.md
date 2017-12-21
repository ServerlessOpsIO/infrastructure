# Infrastructure

This repo contains the infrastructure code for the shared infrastructure at ServerlessOps.  It uses [Serverless Framework](https://serverless.com/) to deploy infrastructure.

## Organization
This repository is structured by resource type instead of end to end configuration.  This makes the code easier to write but has the drawback of not having any ordering or relationships defined in code between resources.  This means you have to know about resource relationships in your own without relying on CloudFormation finding errors during initial validation.  Because not a lot of reconfiguration is expected this should be fine.

To deploy resources, change into the appropriate subdirectory and run:

```
serverless deploy
```

### Stages
Stages are represented by different AWS organization accounts.  Where different configuration is needed between accounts, resources should be isolated in a file named after the stage/account.  The _serverless.yml_ should then have the following:

```yaml
provider:
  name: aws
  stage: ${opt:stage, env:SLS_STAGE, 'dev'}
  profile: ${opt:aws-profile, env:AWS_PROFILE, env:AWS_DEFAULT_PROFILE, 'default'}

resources: ${file(./${self:provider.stage}.yml)}
```

## Choosing Serverless Framework
While Serverless Framework is aimed at being a system and function deployment tool it compiles down into CloudFormation that is deployed.  The decision was made to not introduce another tool for simplicity sake and because solving issues with the DSL of Serverless Framework would potentially be re-applicable to other systems.

