page_title: Running your first sample build
page_description: Setup up documentation for a sample build
page_keywords: getting started, build, quick start, documentation, shippable, continous integration

# Run a sample CI build 

This tutorial walks through the process of running a sample build using Shippable's continuous integration. A Node.js sample application is leveraged for this sample.

You need a GitHub account to complete this tutorial.

* * * 

## Fork our sample project

Fork the following GitHub repositories in order to complete the tutorial:

<https://github.com/shippableSamples/sampleNod> 

* * * 

## Sign in
Login to [Shippable](http://www.shippable.com) using your GitHub credentials. 

* * * 

## Enable a project

The first step is to enable continuous integration for your forked repository.

- On the [Shippable landing page](http://www.shippable.com) , select your subscription from the `Subscriptions` dropdown. This should be the subscription where you forked the repository.

- On the subscription page, click on the `Enable Project` button. If you have not enabled any projects on Shippable, you will be directly be taken to a page with a list of your repositories.
-  Find the samplenod project and click on the `Enable` button.
- If samplenod repo is not shown  in the list, click on the `Sync` button next to the Search box. This syncs your Shippable subscription with your source control account. Find the samplenod project and enable it.

* * * 

## Run a build
Your sample project already has a config file 	`shippable.yml` at the root of the repository.
 
- After enabling the project, you will be redirected to the Project page for samplenod.

- Click the `Build` button to trigger your build.
 
* * * 

## View build status
Congratulations! You have successfully run your first build on Shippable. 

* * * 

## More examples
Check out the stepwise, multi-part series of tutorials that takes you through running CI on a node.js application [hosted on a public repo](http://blog.shippable.com/get-started-with-continuous-integration-for-nodejs-app), [private repo](http://blog.shippable.com/running-ci-on-a-node.js-application-hosted-on-a-private-repository), [configuring unit tests](http://blog.shippable.com/configuring-a-unit-test-to-check-for-errors-on-the-node.js-application) and much, much more.

---
