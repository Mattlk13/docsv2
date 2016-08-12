
# Java
This page explains yml configuration that is specific to Java projects. For a complete yml reference, please read the [Build configuration section](../shippableyml.md)

##yml configuration

The sections below explore sections of the yml that are specific to Java projects. 


###language 


For Java projects, this tag should always be set to java as show below:

```
language: java
```

### runtime
Our official build images for Java come pre-installed with the following versions:

* openjdk6
* openjdk7 (default if no runtime specified)
* oraclejdk7
* oraclejdk8

You can set the runtime to any version(s) using the `jdk` tag:

```
jdk:
  - oraclejdk8
```

If you want to test against several versions of Java, you can specify multiple runtimes. The snippet below will trigger 2 builds, one against each version:

```
jdk:
  - openjdk7
  - oraclejdk7
```

**Important note:** The runtime tag only works with official CI images provided by Shippable. If you are using a custom image for your build, you will need to switch the runtime in the `ci` section of your yml using `update-alternatives`.

### pre_ci and pre_ci_boot

Depending on the `language` and `services` tags in your yml, an official build image is chosen for your build by default, and your build container is started with standard options. The default images for Java builds are explained below.

The pre_ci and pre_ci_boot sections are primarily used in one of the following scenarios:

* You want to use a custom Docker image for your CI 
* You want to override the default options that are used to boot up the default CI image

If you do not want to do either of the above, you should skip these tags in the yml.

#### Default Java images
We have 2 primary build images for Java projects, which should be sufficient for most projects: 

* [dry-dock/u14jav](https://github.com/dry-dock/u14jav) is used if you specify `language: java` in your yml and do not specify a `services` tag. This image contains the following:
	
	* Ubuntu 14.04
	* Java versions openjdk6, openjdk7, oraclejdk7, oraclejdk8
	* Gradle 2.3
	* Apache maven 3.2.5
	* Apache ant 1.9.6
	* Git
	* Basic packages sudo, build-essential, curl, gcc, make, openssl, software-properties-common, wget, nano, unzip, libxslt-dev, libxml2-dev
	* Python packages python-pip, python-software-properties, python-dev
	* Node version 0.10 
	* Python 2.7.6
	* Default Ruby version 
	* awscli
	* google-cloud-sdk 

* [dry-dock/u14javall](https://github.com/dry-dock/u14javall) is used if you specify one or more services and set the language to java in the yml. This image contains the following **in addition** to everything that is listed for the u14java image above:

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
    - mvn clean cobertura:cobertura
```

#### Multi-module Maven builds
When using multi-module (Reactor) builds, please remember to output all the coverage and tests reports to the (top-level) repository directory. This can be tricky, as the Cobertura plugin resolves output directory differently from Surefire plugin. The most straightforward way of dealing with this issue is to define plugin configuration in the top-level module, using shippable/codecoverage path for Cobertura plugin and ../shippable/testresults for Surefire plugin:

```
<plugin>
  <groupId>org.codehaus.mojo</groupId>
  <artifactId>cobertura-maven-plugin</artifactId>
  <version>2.6</version>
  <configuration>
    <format>xml</format>
    <maxmem>256m</maxmem>
    <aggregate>true</aggregate>
    <outputDirectory>shippable/codecoverage</outputDirectory>
  </configuration>
</plugin>
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-surefire-plugin</artifactId>
  <version>2.17</version>
  <configuration>
    <redirectTestOutputToFile>true</redirectTestOutputToFile>
    <reportsDirectory>../shippable/testresults</reportsDirectory>
  </configuration>
  <dependencies>
    <dependency>
      <groupId>org.apache.maven.surefire</groupId>
      <artifactId>surefire-junit4</artifactId>
      <version>2.7.2</version>
    </dependency>
  </dependencies>
</plugin>

```

#### Retrying installation of dependencies
Your dependencies can sometimes fail to install due to network glitches or other external transient factors. You can harden the command for installing dependencies by using `shippable_retry`. We will then retry the command up to 3 times if it returns a non-zero code.


```
build:
  ci:
    - shippable_retry mvn install -DskipTests=true
```

#### Default commands

If the `ci` section is blank, we will run some default commands as explained below.

* If your repository root does not have gradle or maven files, then our Java builder will use Ant. This is the same as the snippet below:

```
build:
  ci:
    - ant test
```
* If your repository has a pom.xml file at the root, then our Java builder will use Maven 3:

```
build: 
  ci:
   - mvn install -DskipTests=true
```

* If your repository has a build.gradle file at the root, then our Java builder will use gradle:

```
build:
  ci:
   - gradle assemble
```

To avoid executing the default command, include a simple command in like `pwd` or `ls` in this section.







