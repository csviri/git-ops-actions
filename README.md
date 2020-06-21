# git-ops-actions

GitOps Actions is a simple, minimal yet general GitOps toolkit. 
It helps you to implement git ops for your target system.
It consists of 2 GitHub Actions:

- **Propagate Version Action**: https://github.com/csviri/git-ops-actions-propagate-version  

  Use this action in your source repository, thus in your CI pipeline, to propagate version
  information  of the built artifact to your Ops repository from your source code repository. 
  See our [source code repo sample.](https://github.com/csviri/git-ops-actions-sample-ci)

- **Promote Action**: https://github.com/csviri/git-ops-actions-promote
  
  Use this action to promote deployment between environments in your Ops Repository. For now, we support a model
  where branches in your Ops repository represents your environments. When we want to promote
  a deployment to the next environment (like from dev to prod or pre-prod), the action creates
  a pull request between branches representing source and target environment. To describe this workflow
  put a `gitopsactions.yaml` file on the root of your Ops Repository. See the [sample Ops repo.](https://github.com/csviri/git-ops-actions-sample-ops-repo)
  
  
The 2 samples from above represents a skeleton of typical workflow:
  1. Artifact is build from source code with a version
  2. The version is propagated to the `dev` environment. Here by adding a version file
     to the ops repository, on dev branch.
  3. A GitHub Action pipeline is executed on dev branch of Ops Repository. As a last step
  the version and all changes are pomoted using a pull request to the `pre-prod` branch.
  This PR is configured to be automatically approved.
  4. The pipelines on `pre-prod` branch is executed, and as just on `dev`, a new PR is created
  for `prod` environment. This is configured to be not automatically approved.
  5. Somebody manually approves the PR to `prod`, the GitHub Action pipeline deploys the app
  to prod environment.    
   
     
Sample environment mapping file from ops repo:

```yaml
promotionMapping:
  dev:
    autoApprove: true
    targetEnvironments: ["pre-prod"]
  pre-prod:
    autoApprove: false
    targetEnvironments: ["prod"]
```  


  
