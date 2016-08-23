page_title: Unified Pipeline Resources
page_description: List of supported resources
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

# syncRepo
The `syncRepo` resource is at the heart of your deployment pipelines. This resource is a pointer to the source control repository containing the files that define your pipelines:  `shippable.resources.yml` and `shippable.jobs.yml`.

When you add a `syncRepo`, Shippable will read the jobs and resources ymls and create your pipeline. We also add a webhook on the source repository that notifies Shippable each time anything in the repository is changed. This webhook notification will automatically sync any changes you make to the jobs and resources ymls and reflect them in your pipeline. 

You need to add at least one syncRepo resource from the Shippable UI. Additional syncRepo resources can be added through the UI or by specifying them in the shippable.resources.yml file in the first syncRepo that was added.

This is the only resource type that can be added from the UI. You should not add the same repository as a sync repo more than once. This can lead to unexpected behavior.

## Adding a syncRepo from the UI

* Go to your Subscription's page and click on `Pipelines`
* Click on the `Resources` pill and then click on `Add resource` at the right
* You will first need to select a subscription integration. This should point to the source control system where the repository containing your pipeline definitions is located. To learn how to create subcription integrations for source control, read the [SCM section of integrations overview page](../../integrations/overview.md#scm). 
* Once you add the integration, you will see a list of repositories in your subscription.
* Select the repository and branch that contains your shippable.jobs.yml and shippable.resources.yml files
* Name your sync repository and click on `Save`. This should seed your pipeline.

If you click on the SPOG tab, you should see a visualization of the the jobs and resources from your pipeline.

## Adding a syncRepo through the yml
You will always need to add at least one syncRepo through the UI. Subsequent syncRepos can be either added through the UI or you can include them in the `shippable.resources.yml`of the first syncRepo:

```
- name: prod-repo                           #required
  type: syncRepo                            #required
  integration: avinci-gh                    #required, source control integration
  pointer:
    sourceName: avinci/prod                 #required, org/repo
    branch: master                          #optional, default master
```

This will create a resource of type `syncRepo` with the name `prod-repo`. It uses an scm integration named `avinci-gh` which is an integration of type scm. To learn how to create subcription integrations for source control, read the [SCM section of integrations overview page](../../integrations/overview.md#scm). The repo name is `prod` and belongs to `avinci` org. The branch to look for resource and job definitions is `master`

* `name` should be an easy to remember text string. This will appear in the visualization of this resource in the SPOG view and the list of resources in the Pipelines `Resources` tab. It is also used to refer to this resource in the jobs yml.
 
* `type` is always set to syncRepo.

* `integration` should be the name of the integration that connects to the Source Control provider where the repository is located. To learn how to create integrations for a specific Source Control Provider, please select from the list below and read the **Adding an integration** section on that page: 

	- [GitHub](../../integrations/scm/github/)
	- [Bitbucket](../../integrations/scm/bitbucket/)
	- [Github Enterprise](../../integrations/scm/githubEnterprise/)
	- [Bitbucket Server (formerly Stash)](../../integrations/scm/bitbucketServer/)
	- [Gitlab/GitlabServer](../../integrations/scm/gitlab/)


* `pointer` section provides information about the repository and branch where the configuration files are located.
	* `sourceName` is the fully qualified name of the repository in the format **org/repo**
	* `branch` specifies the branch. If not specified, it defaults to `master`.
	


