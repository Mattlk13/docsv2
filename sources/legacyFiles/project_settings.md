page_title: Shippable Project Settings
page_description: How to configure Shippable CI Project Settings
page_keywords: project settings, CI/CD, shippable CI, documentation, shippable, config, yml, build configuration, build timeout, build badge

# Project Settings

This page documents the configuration for your build that is set up in the UI. It also covers the additional settings that you can set up via the Shippable YML.

Check out [How To Run a Build](build_case2) for guidance on running a build with the basic YML settings.

## How do you get here?

- Login to [Shippable](http://shippable.com)
- Click on **CI** on the Shippable Landing Page and choose the appropriate subscription
- This brings you to the [Subscriptions Dashboard](ci_dashboard). If your project is enabled, it will show up on the dashboard. If not, click on the ![add icon](images/enable_icon.gif) to enable your project
- Click on the project
- Select the `Settings` tab

## Project Settings on the UI

Any settings on the UI applies to all builds for that project.

---

### Docker Options

This tells us how you'd like to use Docker within your Shippable CI build.  Click 'Edit' at the bottom of the section in order to change the settings.

<img src="../images/docker-options.png" alt="Docker Options" style="width:800px;"/>

#### CI Image:

This setting specifies what Docker image to use for the build.  The options are:  

**Use default Shippable Image** (default selection)  

  - This option will use the default Shippable image ('minv2') as the base image for your build container
  - Inputs required: **Push Image to Hub, Cache Image, Lighthouse Notifications**

**Build from a Dockerfile**  

  - This option will build the image to use for your CI container from a Dockerfile you provide in the root of your repo
  - Inputs required: **New Image Name, New Image Tag, Source Code Path, Push Image to Hub, Lighthouse Notifications**

**Pull image from a Registry**  

  - This option will use a base image that you specify.  This can be any image including a custom image you've built and stored in a registry or one of the language-specific images Shippable provides in [Docker Hub](https://hub.docker.com/u/shippableimages/).
  - Inputs required: **Pull Image From, Push Image to Hub, Cache Image, Lighthouse Notifications**

For example, if you select 'Build from a Dockerfile', you should see the following on your screen:  

<img src="../images/docker-options-build-dockerfile.png" alt="Build from a Dockerfile" style="width:800px;"/>

Once you've selected a value for 'CI image', you'll need to specify the input values.  Here's what each input represents:

#### Cache Image:

Unless you specify, Shippable does not cache dependencies between builds. Each build will run on a fresh minion and as soon as the build finishes execution, the minion will be deleted. However, we also understand that installing dependencies for each build will take more time and it affects your build speed. Hence the caching feature.

By default, Cache is set to OFF.  Turn this ON if you want to save build sessions between builds.  

  - Before the build, we will check for the **Cache Image** flag and if it is ON, the entire minion will be cached if the build succeeds and the cached minion will be reused for further builds.
  - Caching is done per build host, so it might take a few builds for all our hosts to get the cached minion. Additional details on caching can be found on our [blog](http://blog.shippable.com/container-caching).

#### Lighthouse Notifications:

You will see an option to turn ON Lighthouse for an image that you push to the registry. If you set this to ON, you will be notified every time the image gets updated in the registry.

When you turn ON the Lighthouse option, remember to select the notification integration for the project. Scroll down to Project Integrations and select the appropriate Notification Integration.

See our [Lighthouse Page](lighthouse.md) for details on how to use the feature.

Read [how to set up a Notification Integration](integrations.md)

#### New Image Name:

Provide the full path of the registry location where you would like to push the build output.

Required format for **Push Image Name**:

Amazon EC2 Container Registry: **AWS ACCOUNT ID.dkr.ecr.us-east-1.amazonaws.com/OPTIONAL NAMESPACE/IMAGE NAME**

  - Example: `999999999999.dkr.ecr.us-east-1.amazonaws.com/aye0aye/micro-api`

Docker Hub: **REPOSITORY NAME/IMAGE NAME**

  - Example: `aye0aye/micro-api`

Google Container Registry: **gcr.io/YOUR GOOGLE PROJECT ID>/IMAGE NAME**

  - Example: `gcr.io/aye0aye/micro-api`
  - You can also use region-specific hostnames, e.g. us.gcr.io, eu.gcr.io, etc. (see [Google's documentation](https://cloud.google.com/container-registry/docs/#pushing_to_the_registry) for more)

Quay: **quay.io/REPOSITORY NAME/IMAGE NAME**

  - Example: `quay.io/aye0aye/micro-api`

#### New Image Tag:

Click on the dropdown and choose the appropriate value to use to tag your images. You can choose between:  

- **default:**  This is the default selection and will tag your images with <branch>.<build number>
- **commitsha:** This will tag your images with the commitsha from your source code repo
- **custom:**  This will tag your images with a custom tag that you specify, e.g. 'latest'

By default, we use build numbers as tags. Check out our [blog](http://blog.shippable.com/immutable-containers-with-version-tags-on-docker-hub) on immutable containers to know why. More details can be found [here](docker_registries.md).

#### Pull Image From:

This will be required only if you've chosen 'Pull image from a Registry'. This is where you specify a particular image that you want Shippable to use as the base image for your project.

Example: `shippableimages/ubuntu1204_nodejs`

You can pull any public image or private image to which you have access. Read our guide on [how to use Docker Registries](docker_registries.md) to learn more about pulling images from various registries.

The default image, shippable/minv2, comes installed with popular versions of all supported languages, tools and services. However, you might prefer starting with a small image that only has versions of your language installed (we highly recommend this as minv2 is quite large!). To help with this, we have open sourced basic images for all supported languages. These images only come with popular versions of a language and are NOT pre-installed with any tools, add-ons or services.

Check out our [language help page](languages.md) for language-specific info about build images.

Our build images are available on Docker Hub in the [shippableImages account](https://registry.hub.docker.com/repos/shippableimages/) . Dockerfiles
for these images are in our [GitHub repository](https://github.com/shippableImages).

> **Note**
>
> Don't forget to set your Hub Integration to the correct registry if you are pulling a private repo from Docker Hub, Amazon ECR, GCR, or Quay. Read the [Project Integrations section](project_settings/#project-integrations) to learn how.
>

#### Push Image to Hub:

If you select 'Yes', Shippable will push a Docker image to the registry you specify at the conclusion of a successful build.

If you have chosen 'Use default Shippable Image' or 'Pull image from Registry', you'll have the option to either push the image used to perform your CI build (select 'Push') or specify that Shippable should build a new image based on a Dockerfile in the root of your directory and push that image (select 'New Build').  If you have chosen 'Build from Dockerfile', we used the Dockerfile in the root of your directory to build an image to use to perform your CI build and we will push that image.

You'll be required to provide the full path of the registry location where you would like to push the build output.

Required format for **Push Image Name**:

Amazon EC2 Container Registry: **AWS ACCOUNT ID.dkr.ecr.us-east-1.amazonaws.com/OPTIONAL NAMESPACE/IMAGE NAME**

  - Example: `999999999999.dkr.ecr.us-east-1.amazonaws.com/aye0aye/micro-api`

Docker Hub: **REPOSITORY NAME/IMAGE NAME**

  - Example: `aye0aye/micro-api`

Google Container Registry: **gcr.io/YOUR GOOGLE PROJECT ID>/IMAGE NAME**

  - Example: `gcr.io/aye0aye/micro-api`
  - You can also use region-specific hostnames, e.g. us.gcr.io, eu.gcr.io, etc. (see [Google's documentation](https://cloud.google.com/container-registry/docs/#pushing_to_the_registry) for more)

Quay: **quay.io/REPOSITORY NAME/IMAGE NAME**

  - Example: `quay.io/aye0aye/micro-api`

#### Source Code Path:

Option shown only if you've selected 'Build from a Dockerfile'.

This is where you specify the location in your CI build container where the source code should be copied. For example, in our [sample docker build project](https://github.com/shippableSamples/docker-build-nodejs), it is `/src`.

------

### Project Integrations

<img src="../images/project-integrations.png" alt="Project Integrations" style="width:800px;"/>

#### Hub Integration

You will need this if you are pulling from or pushing to a docker registry, such as Docker Hub or Google Container Registry.

PRE-REQUISITE: Before setting an integration at the project level, you need to connect the registry at an account level. The [Integrations guide](integrations.md) tells you how to create account level integrations that you can use across projects.

When you click on the Hub Button, you will see the list of integrations that are available for you to use. This may also include integrations created by owners in other subscriptions you belong to.

Choose the appropriate integration name to connect to the registry of your choice or select 'Create Integration' to create a new one.

#### Notification Integration

This integration is currently used to tell the project the notification integration you want to use for Lighthouse if you want to be notified of changes in the image you are pushing.

The [Integrations guide](integrations.md) will tell you how to create the notification integration at the account level.

>**Note**
>
>If there are no integrations created at the account level, you will not see any options when clicking the dropdown on either HUB or NOTIFICATION

---

### Deployments

Select from the dropdown to enable integrations to deploy to third party services.

<img src="../images/deployment-integrations.png" alt="Deployment Integrations" style="width:800px;"/>

---

### Encrypt Environment Variables

<img src="../images/encrypt.png" alt="Encrypt Variables" style="width:800px;"/>

Shippable allows you to encrypt the environment variable definitions and
keep your configurations private using **secure** tag.

Enter the env variable and its value in the text box as shown below.

```
name=abc
```

Click on the encrypt button and copy the encrypted output string and add
it to your yml file as shown below:

```yaml
env:
  secure: <encrypted output>
```

To encrypt multiple environment variables and use them as part of a
single build, enter the environment variable definitions in the text box
as shown below

```
name1="abc" name2="xyz"
```

This will give you a single encrypted output that you can embed in your
yml file.

You can also combine encrypted output and clear text environments using
**global** tag.

```yaml
env:
  global:
    - FOO="bar"
    - secure: <encrypted output>
```

To encrypt multiple environment variables separately, configure your yml
file as shown below:

```yaml
env:
  global:
    #encrypted output of first env variable
    - secure: <encrypted output>
    #encrypted output of second env variable
    - secure: <encrypted output>
  matrix:
    #encrypted output of third env variable
    - secure: <encrypted output>
```

> **Note**
>
> Due to the security risk of exposing your secure variables, we do not
> decrypt secure variables for pull request from the forks of public
> projects. Secure variable decryption is limited to the pull request
> triggered from the branches on the same repository. And the decrypted
> secured variables are also not displayed in the script tab for
> security reasons.

------

## Project Settings via the YML

In this section, we will walk through the settings or configuration for your build that's set in the YML.

**By default, we will build all branches of the project as long as there is a shippable.yml in the root of every branch. The YML file is specific to a branch.**

### Build matrix

This is a powerful feature that Shippable has to offer. You can
trigger multiple different test passes for a single code push. You might
want to test against different versions of ruby, or different aspect
ratios for your Selenium tests or best yet, just different jdk versions.
You can do it all with Shippable's matrix build mechanism.

```yaml
rvm:
  - 1.8.7 # (current default)
  - 1.9.2
  - 1.9.3
  - rbx
  - jruby
  - ruby-head
  - ree
gemfile:
  - gemfiles/Gemfile.rails-2.3.x
  - gemfiles/Gemfile.rails-3.0.x
  - gemfiles/Gemfile.rails-3.1.x
  - gemfiles/Gemfile.rails-edge
env:
  - ISOLATED=true
  - ISOLATED=false
```

The above example will fire 36 different builds for each push. Whoa!
Need more minions?

### Environment Variables

#### Standard environment variables

The following environment variables are available for every build. You
can use these in your scripts if required:

- BRANCH : Name of branch being built
- BASE_BRANCH : Name of the target branch into which the pull request
  changes will be merged
- BUILD_NUMBER : Build number for current build
- BUILD_URL : Direct URL link to the build output
- CI : true
- CONTINUOUS_INTEGRATION : true
- COMMIT : Commit id that is being built and tested
- COMPARE_URL : A link to GitHub/BitBucket's comparision view for the
  push
- DEBIAN_FRONTEND : noninteractive
- HEAD_BRANCH: Name of the most recently committed branch
- JOB_ID : id of job in Shippable
- LANG : en_US.UTF-8
- LAST_SUCCESSFUL_BUILD_TIMESTAMP : Timestamp of the last
  successful build in seconds. This will be set to **false** for the
  first build or for the build with no prior successful builds
- LC_ALL : en_US.UTF-8
- LC_CTYPE : en_US.UTF-8
- MERB_ENV : test
- PATH : \$HOME/bin:\$PATH
- PULL_REQUEST : Pull request number if the job is a pull request. If
  not, this will be set to **false**
- RACK_ENV : test
- RAILS_ENV : test
- REPO_NAME : Name of the repository currently being built
- REPOSITORY_URL : URL of your GitHub or Bitbucket repository
- SERVICE_SKIP : false
- SHIPPABLE : true
- SHIPPABLE_ARCHIVE : true
- SHIPPABLE_BUILD_ID : id of build in Shippable
- SHIPPABLE_MYSQL_BINARY : "/usr/bin/mysqld_safe"
- SHIPPABLE_MYSQL_CMD : "\$SHIPPABLE_MYSQL_BINARY"
- SHIPPABLE_POSTGRES_VERSION : "9.2"
- SHIPPABLE_POSTGRES_BINARY :
  "/usr/lib/postgresql/\$SHIPPABLE_POSTGRES_VERSION/bin/postgres"
- SHIPPABLE_POSTGRES_CMD : "sudo -u postgres
  \$SHIPPABLE_POSTGRES_BINARY -c
  "config_file=/etc/postgresql/\$SHIPPABLE_POSTGRES_VERSION/main/postgresql.conf""
- SHIPPABLE_VE_DIR : "\$HOME/build_ve/python/2.7"
- USER : shippable

#### User Specified Environment Variables

You can set your own environment variables in the yml. Every statement
of this command will trigger a separate build with that specific version
of the environment variables.

```yaml
# environment variable
env:
 - FOO=foo BAR=bar
 - FOO=bar BAR=foo
```

> **Note**
>
> Env variables can create an exponential number of builds when combined
> with `jdk` & `rvm , node_js etc.` i.e. it is multiplicative

In this setting **4 individual builds** are triggered in a build group

```yaml
# npm builds
node_js:
  - 0.10.24
  - 0.8.14
env:
  - FOO=foo BAR=bar
  - FOO=bar BAR=foo
```

>**NOTE**
>
> See the prior section on Encrypt Variables on how to secure any environment variables

### Command collections
 `shippable.yml` supports collections under each tag. This is nothing more than YML functionality and we will run it one command at a time.

```yaml
# collection scripts
script:
 - ./minions/do_something.sh
 - ./minions/do_something_else.sh
```

In the example above, our minions will run `./minions/do_something.sh`
and then run `./minions/do_something-else.sh`. The only requirement is
that all of these operations return a `0` exit code. Else the build will
fail.

### Retrying npm install

Sometimes npm install may fail due to the intermittent network issues
and affects your build execution. To avoid this, **shippable_retry**
 will try to install the command again. It will check the return
code of a command and if it is non-zero, then it will re-try to install
up to three times.

**shippable_retry** functionality is available for all default
installation commands and it will re-try to install on failure. You can
also use this functionality for any custom installation from external
resources. For example:

```yaml
before_install:
    - shippable_retry sudo apt-get update
    - shippable_retry sudo apt-get install something
```

### Git submodules

Shippable supports git submodules. This is a cool functionality of
breaking your projects down into manageable chunks. We automatically
initialize the `.gitmodules` file in the root of the repo.

> **Note**
>
> If you are using private repos, add the deploy keys so that our minion
> ssh keys are allowed to pull from the repo. This can be done via
> shippable.com

If its your own public repos then do this

```python
# for public modules use
git://github.com/someuser/somelibrary.git

# for private modules use
git@github.com:someuser/somelibrary.git
```

If you would like to turn submodules off completely -

```yaml
# for public modules use
git:
 submodules: false
```

### Include/Exclude Branches

By default, Shippable builds all branches for enabled repositories as
long as they have a shippable.yml at the root.

You can change this build only specific branches using the include and
exclude sections in your yml. The specific branch that is being included
or excluded needs to have this configuration, and not just the master
branch.

This is because Shippable works as follows - we get a webhook for an
enabled repository letting us know something has changed in a specific
branch. We read the shippable.yml from that branch and then trigger a
build based on that. So if your shippable.yml in the develop branch does
not contain the exclude section, we will trigger a build irrespective of
what's in the yml in master branch.

Here is a sample of the include/exclude config -

```yaml
# exclude
branches:
  except:
    - test1
    - experiment2

# include
branches:
  only:
    - stage
    - prod
```

### Exclude a version

It is also possible to exclude a specific version using exclude tag.
Configure your yml file as shown below to exclude a specific version.

```yaml
matrix:
  exclude:
    - rvm: 1.9.2
```

### Include a version

You can also configure your yml file to include entries into the matrix
with include tag.

```yaml
matrix:
  include:
    - rvm: 2.0.0
      gemfile: gemfiles/Gemfile.rails-3.0.x
      env: ISOLATED=false
```

### Allow-failures

Allowed failures are items in your build matrix that are allowed to fail
without causing the entire build to be shown as failed. You can define
allowed failures in the build matrix as follows:

```yaml
matrix:
  allow_failures:
    - rvm: 1.9.3
```

> **Note**
>
> When using matrix vars in your YML, please note matrix:include builds ONLY use env vars that are explicitly
> specified in the include item. Global env variables that are outside of the include item are NOT inherited.
> You will need to specify these separately under each matrix.

---

### Test Results

To set up test result visualization for a repository.

- Output test results to shippable/testresults folder.
- Make sure test results are in junit format.

For example, here is the .yml file for a Python repo -

```yaml
before_script: mkdir -p shippable/testresults
script:
  - nosetests python/sample.py --with-xunit --xunit-file=shippable/testresults/nosetests.xml
```

Examples for other languages can be found in our
[Code Samples](languages/).

### Code Coverage Visualization

To set up code coverage result visualization for a repository.

- Output code coverage output to shippable/codecoverage folder.
- Make sure code coverage output is in cobertura xml format.

For example, here is the .yml file for a Python repo -

```yaml
before_script: mkdir -p shippable/codecoverage
script:
  - coverage run --branch python/sample.py
  - coverage xml -o shippable/codecoverage/coverage.xml python/sample.py
```

Examples for other languages can be found in our Code Samples.

---

### Notifications

Shippable primarily supports email and irc notifications and these can
can be configured in your yml file. To send Slack notifications, please
check out our [blog post](http://blog.shippable.com/devops-chat-a-simple-way-to-use-slack-notifications-with-shippable).
To send HipChat notifications, check out our [sample project for hipchat notifications](https://github.com/shippableSamples/sample-hipchat-notifications).

By default, we send email notifications to the last committer when a
build fails, or the status changes from failed to passed.

You can change the default settings for email notifications by
configuring the notifications section of your yml. You can specify the
email address(es) where you want to receive notification as well as the
criteria for when you want notifications to be sent.

#### Email notifications

To send notifications to specific email addresses, replace the sample
email addresses below with the recipients' email ids in your
`shippable.yml` file.

```yaml
notifications:
    email:
        - exampleone@org.com
        - exampletwo@org.com
```

You can also specify when you want to get notified by setting the values
for on_success and on_failure keys to change|always|never. Change
means you want to be notified only when the build status changes on the
given branch. Always and never mean you want to be notified always or
never respectively.

```yaml
notifications:
     email:
         recipients:
             - exampleone@org.com
             - exampletwo@org.com
         on_success: change
         on_failure: always
```

If you do not want to get notified for any reason, you can configure
email notifications to false.

```yaml
notifications:
   email: false
```

#### IRC notifications

You can also configure yml file to send build notifications to your IRC
channels.

- To specify single channel

```yaml
notifications:
   irc:  "chat.freenode.net#channel1"
```

- You can also specify multiple server channels in yml file. The
  following formats are supported:

```yaml
notifications:
  irc:
    - "chat.freenode.net#channel1"
    - "chat.freenode.net#channel2"
    - "server1#channel3"
```

```yaml
notifications:
  irc:
   channels:
     - "chat.freenode.net#channel1"
     - "chat.freenode.net#channel2"
     - "server1#channel3"
```

- By default, We will always send build notifications to the mentioned
  channels in yml. **on_success** and **on_failure** are not yet
  configurable.
- IRC notifications are turned off by default for pull request builds.
  However, you can change the default settings by adding
  **pull_requests: true** tag in your yml as shown below.

```yaml
notifications:
  irc:
   pull_requests: true
   channels:
```
---

### Services

Shippable offers a host of pre-installed services to make it easy to run
your builds. In addition to these you can install other services also by
using the `install` tag of `shippable.yml`.

All the services are turned off by default and can be turned on by using
the `services:` tag.

#### MongoDB

```yaml
# Mongo binds to 127.0.0.1 by default
services:
 - mongodb
```

Sample PHP code using
[mongodb](https://github.com/shippableSamples/sample_php_mongo) .

#### MySQL

```yaml
# MySQL binds to 127.0.0.1 by default and is started on boot. Default username is shippable with no password
# Create a DB as part of before script to use it

before_script:
    - mysql -e 'create database myapp_test;'
```

Sample javascript code using
[mysql](https://github.com/shippableSamples/sample_node_mysql).

#### SQLite3

SQLite is a software library that implements a self-contained,
serverless, zero-configuration, transactional SQL database engine. So
you can use SQLite, if you do not want to test your code behaviour with
other databases.

Sample python code using
[SQLite](https://github.com/shippableSamples/sample_python_sqllite).

#### Elastic Search

```yaml
# elastic search is on default port 9200
services:
    - elasticsearch
```

Sample python code using [Elastic Search](https://github.com/shippableSamples/sample_python_elasticsearch).

#### Memcached

```yaml
# memcached runs on default port 11211
services:
    - memcached
```

Sample python code using
[Memcached](https://github.com/shippableSamples/sample_python_memcache) .

#### Redis

```yaml
# redis runs on default port 6379
services:
    - redis
```

Sample python code using
[Redis](https://github.com/shippableSamples/sample_python_redis).

#### Neo4j

```yaml
#neo4j runs on default port 7474
services:
 - neo4j
```

Sample javascript code using
[Neo4j](https://github.com/shippableSamples/sample_node_neo4j) .

#### Cassandra

```yaml
# cassandra binds to the default localhost 127.0.0.1 and is not started on boot.
services:
  - cassandra
```

Sample ruby code using
[Cassandra](https://github.com/shippableSamples/sample_ruby_cassandra) .

#### CouchDB

```yaml
# couchdb binds to the default localhost 127.0.0.1 and runs on default port 5984. It is not started on boot.
services:
  - couchdb
```

Sample ruby code using
[CouchDB](https://github.com/shippableSamples/sample-ruby-couchdb) .

#### RethinkDB

```yaml
# rethinkdb binds to the default localhost 127.0.0.1 and is not started on boot.
services:
  - rethinkdb
```

Sample javascript code using
[RethinkDB](https://github.com/shippableSamples/sample-node-rethinkdb).

#### RabbitMQ

```yaml
# rabbitmq binds to 127.0.0.1 and is not started on boot. Default vhost "/", username "guest" and password "guest" can be used.
services:
  - rabbitmq
```

Sample python code using
[RabbitMQ](https://github.com/shippableSamples/sample_python_rabbitmq) .

---

### Addons

#### Firefox

We support different firefox versions like "18.0", "19.0", "20.0",
"21.0", "22.0", "23.0", "24.0", "25.0", "26.0", "27.0", "28.0", "29.0".
To select a specific firefox version, add the following to your
shippable.yml file.

```yaml
addons:
   firefox: "21.0"
```

#### Custom Host Name

You can also set up custom hostnames using the **hosts** addons. To set
up the hostnames in /etc/hosts file, add the following to your
shippable.yml file.

```yaml
addons:
   hosts:
    - google.com
    - asdf.com
```

#### PostgreSQL

```yaml
# Postgre binds to 127.0.0.1 by default and is started on boot. Default username is "postgres" with no password
# Create a DB as part of before script to use it

before_script:
  - psql -c 'create database myapp_test;' -U postgres
```

Sample java code using
[PostgreSQL](https://github.com/shippableSamples/sample_java_postgres).

We support PostgreSQL 9.1, 9.2 and 9.3 versions and by default, version
9.2 is installed on our minions. Configure your yml file using
**PostgreSQL** addons to select different versions. Add the following to
your yml file to select the version 9.3.

```yaml
addons:
 postgresql : "9.3"
```

PostGIS 2.1 packages are pre-installed in our minions along with the
PostgreSQL versions 9.1, 9.2 and 9.3.

#### Selenium

Selenium is not started on boot. You will have to enable it using
**services** tag and start xvfb (X Virtual Framebuffer) on display port
99.0, so that all your test suites will run on the server without a
display. Configure your yml file as shown below to start selenium server
on firefox.

```yaml
addons:
  firefox: "23.0"

services:
  - selenium

before_script:
  - "export DISPLAY=:99.0"
  - "/etc/init.d/xvfb start"
```

Selenium **2.40** is started by default. You can also select a different
version of selenium using **addons** tag. The following versions are
supported:

- 2.39
- 2.40
- 2.41
- 2.42
- 2.43
- 2.44

Choose the required version and add it to your shippable.yml file as
shown below

```yaml
addons:
  selenium: "2.43"
```

This will download the required version. You will have to include
**services** tag in your yml file to start the selenium server using the
downloaded version. Configure your yml file as shown below to start
selenium server **2.43** on firefox.

```yaml
#specify required selenium and firefox version
addons:
  selenium: "2.43"
  firefox: "27.0"

#start the selenium server
services:
  - selenium

before_script:
  - "export DISPLAY=:99.0"
  - "/etc/init.d/xvfb start"
```

Sample javascript code using
[Selenium](https://github.com/shippableSamples/sample_node_selenium) .



## Run a Build

Read our guide on [how to run a build](build_case2) to learn how to use these settings within the context of running a build.
