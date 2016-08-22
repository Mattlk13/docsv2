page_title: Unified Pipeline Jobs
page_description: List of supported jobs
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

# manifest

A `manifest` is versioned immutable design time definition of a unit of deployment for your application. Everything in a manifest is always deployed as a whole on to a single node(virtual machine, physical machine etc). Manifests are also scaled as a whole, so creating multiple replicas of a manifest leads to multiple copies of the entire manifest.

Depending on your architecture and requirements, your unit of deployment can be a service, microservice, or even the entire application. If you need your apps/service/microservice to be separately deployed, you need to separate different manifests for each.

Manifest jobs are used to generate a new version of the manifest each time anything in the manifest changes.

Manifest jobs can be of 3 types:

* [Single package manifest](#single): The manifest definition contains only one deployable image. 
* [Multi package manifest](#multi): The manifest definition contains more than one deployable image. All services in the manifest are deployed on the same node and scaled together.
* [Combination manifest](#combination): The manifest is a combination of multiple separately defined manifests. Services in a combination manifest can be deployed and scaled independently.

---
<a name="single"></a>
## Single package manifest pattern

A single package manifest has only one input `image` resource. If your microservices/services in your application are decoupled and versioned, then you might want to independently deploy and manage them by using single package manifests.

<img src="../../images/jobs/singlePackageManifest.png" alt="Single package manifest" style="width:500px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

A manifest is configured in `shippable.jobs.yml` as shown below:

```
- name: <string>                             	#required
  type: manifest                             	#required
  steps:
      - IN: <image>                       		#required
        versionName: <string>           		#optional
      - IN: <dockerOptions>                   	#optional
      - IN: <params>                      		#optional
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is always set to manifest
* One `image` resource is mandatory an an input for a manifest job. Please read documentation on how to [define an image resource](../resources/image/) in your resources yml.
	* By default, the latest version of the image resource will be used to generate the manifest. If you want to pin a specific version of the image, you can do so by including the `versionName` or `versionNumber` tags. 
* `dockerOptions` is an optional tag and customizes the memory, cpu shares, port mappings, etc. Read more on [dockerOptions resource](../resources/dockerOptions/).
* `params` is an optional tag and adds a list of environment params required for the manifest. This can include any key value pairs, for example database connection details. Read more on [params resource](../resources/params/).
* `replicas` is an optional input resource that lets you scale the number of instances of your manifest that you want to deploy. The default value for replicas is 1. Read more on [replicas resource](../resources/replicas/).

---
<a name="multi"></a>
## Multi package manifest pattern
There are some cases where your services are not completely decoupled. For example, your UI component might be tightly coupled with your caching component. In those cases, a manifest with 2 different images might be required. This is defined with the multi package manifest pattern.

<img src="../../images/jobs/multiPackageManifest.png" alt="Multi package manifest" style="width:500px;vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

A multi-package manifest is configured in `shippable.jobs.yml` as shown below:

```
- name: <string>                             	#required
  type: manifest                            	#required
  steps:
      - IN: <image>                       		#required
        versionName: <string>            		#optional
      - IN: <image>                        	#required
        versionNumber: <number>           		#optional
      - IN: <dockerOptions>                	#optional
      - IN: <replicas>							#optional
      - IN: <params>                      		#optional
        applyTo:
          - <image>
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is always set to manifest
* You can define as many `image` resources as needed. Please read documentation on how to [define an image resource](../resources/image/) in your resources yml.
	* By default, the latest version of the image resource will be used to generate the manifest. If you want to pin a specific version of the image, you can do so by including the `versionName` or `versionNumber` tags. 
* `dockerOptions` is an optional tag and customizes the memory, cpu shares, port mappings, etc. Read more on [dockerOptions resource](../resources/dockerOptions/).
	* 	By default, values specified in dockerOptions apply to all images in the manifest. If you want the custom values to only apply to specific images, use the `applyTo` tag and provide a list of images you want to apply them to. 
* `params` is an optional tag and adds a list of environment params required for the manifest. This can include any key value pairs, for example database connection details. Read more on [params resource](../resources/params/).
	* 	By default, values specified in params applies to all images in the manifest. If you want them to only apply to specific images, use the `applyTo` tag and provide a list of images you want to apply them to.
* `replicas` is an optional input resource that lets you scale the number of instances of your manifest that you want to deploy. The default value for replicas is 1. Read more on [replicas resource](../resources/replicas/).


---

##manifest tutorials
[Using a combination manifest pattern](../../tutorials/usingCombinationManifests/])

