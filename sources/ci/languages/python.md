
# Python
This page explains yml configuration that is specific to Python projects. For a complete yml reference, please read the [Build configuration section](../shippableyml.md)

##yml configuration

The sections below explore sections of the yml that are specific to Python projects. 


###language 


For Python projects, this tag should always be set to python as show below:

```
language: python
```

### runtime
Our official build images for Python come pre-installed with the following versions:

* 2.6
* 2.7.6 (default if no runtime specified)
* 3.2
* 3.3
* 3.4
* 3.5
* pypy
* pypy3

You can set the runtime to any version(s) using the `python` tag:

```
python:
  - 3.2
```

If you want to test against several versions of Python, you can specify multiple runtimes. The snippet below will trigger 2 builds, one against each version:

```
python:
  - 3.2
  - 3.3
```

**Important note:** The runtime tag only works with official CI images provided by Shippable. If you are using a custom image for your build, you will need to switch the runtime in the `ci` section of your yml using virtualenv.

### pre_ci and pre_ci_boot

Depending on the `language` and `services` tags in your yml, an official build image is chosen for your build by default, and your build container is started with standard options. The default images for Python builds are explained below.

The pre_ci and pre_ci_boot sections are primarily used in one of the following scenarios:

* You want to use a custom Docker image for your CI 
* You want to override the default options that are used to boot up the default CI image

If you do not want to do either of the above, you should skip these tags in the yml.

#### Default Python images
We have 2 primary build images for Python projects, which should be sufficient for most projects: 

* [dry-dock/u14pyt](https://github.com/dry-dock/u14pyt) is used if you specify `language: python` in your yml and do not specify a `services` tag. This image contains the following:
	
	* Ubuntu 14.04
	* Python versions 2.6, 2.7, 3.2, 3.3, 3.4, 3.5, pypy, pypy3 
	* Pip 7.1.2
	* Virtualenv 13.1.2
	* Python pre-reqs libxml2, libxml2-dev, libxslt1.1, libxslt1-dev, libffi-dev, libssl-dev
	* Python packages python-pip, python-software-properties, python-dev
	* Git
	* Basic packages sudo, build-essential, curl, gcc, make, openssl, software-properties-common, wget, nano, unzip, libxslt-dev, libxml2-dev
	* Default Java versions: default-jre, default-jdk, openjdk-6, oracle jdk 7  
	* Node version 0.10 
	* Default Ruby version 
	* awscli
	* google-cloud-sdk 

* [dry-dock/u14pytall](https://github.com/dry-dock/u14pytall) is used if you specify one or more services and set the language to python in the yml. This image contains the following **in addition** to everything that is listed for the u14pyt image above:

	* couchdb 1.6
	* elasticsearch 1.5
	* neo4j 2.2
	* memcached 1.4
	* mongodb 3.0
	* mysql 5.6
	* postgres 9.4
	* rabbitmq 3.5
	* redis 3.0
	* rethinkdb 2.0
	* riak
	* selenium 2.52
	* sqllite 3


If these official images do not satisfy your requirements, you can do one of 2 things:

- Continue using official images and include commands to install any missing dependencies or packages in your yml
- Use a custom build image that contains exactly what you need for yout CI
	
#### Using a custom build image
If you do decide to use a custom CI image, you will need to configure the `pre_ci_boot` section and optionally, the `pre_ci` section if you're also building the CI image as part of the workflow. Details on how to configure this are available in the [`pre_ci` and `pre_ci_boot` sections of the Build configuration page](../shippableyml.md#build). 

### ci
The `ci` section should contain all commands you need for your `ci` workflow. Commands in this section are executed sequentially. If any command fails, we exit this section with a non zero exit code.

#### Installing dependencies
If needed, you can install your project dependencies using the `pip` command in this section:

```
build:
  ci:
    - "pip install -r requirements.txt --use-mirrors"
```

#### Test and code coverage
You can view your test and code coverage results in a consumable format and drill down further to find out which tests failed or which sections of your code were not covered by your tests.

Your tests results data needs to be in junit format and your code coverage results need to be in cobertura format in order to see these visualizations. Test and code coverage results need to be saved to shippable/testresults and shippable/codecoverage folders so that we can parse the reports.

Sample yml snippet using nose and python coverage:

```  
build:
  ci:
    #Create folders for test and code coverage
    - mkdir -p shippable/testresults
    - mkdir -p shippable/codecoverage
    
    #Run test and code coverage and output results to the right folder
    - nosetests test.py --with-xunit --xunit-file=shippable/testresults/nosetests.xml
    - which python && coverage run --branch test.py
    - which python && coverage xml -o shippable/codecoverage/coverage.xml test.py
```

#### Test against multiple versions of Django 

You can test against multiple versions of DJANGO by setting the env key and then install the required dependencies for it in the ci section.

```
env:
- DJANGO_VERSION=1.2.7
- DJANGO_VERSION=1.3.7
- DJANGO_VERSION=1.4.10

build:
  ci:
    - pip install -q mock==0.8 Django==$DJANGO_VERSION
    - pip install . --use-mirrors
```
Please note that the config above tests against several versions, and will trigger a build matrix consisting of 3 builds.

#### Retrying installation of dependencies
Your dependencies can sometimes fail to install due to network glitches or other external transient factors. You can harden the command for installing dependencies by using `shippable_retry`. We will then retry the command up to 3 times if it returns a non-zero code.


```
build:
  ci:
    - shippable_retry pip install -r requirements.txt --use-mirrors
```

#### Default commands

If the `ci` section is blank, we will run the default command shown in the yml snippet below:

```
build:
  ci:
    - if [ -f $SHIPPABLE_BUILD_DIR/requirements.txt ]; then pip install -r $SHIPPABLE_BUILD_DIR/requirements.txt; fi
```

To avoid executing the default command, include a simple command in like `pwd` or `ls` in this section.






