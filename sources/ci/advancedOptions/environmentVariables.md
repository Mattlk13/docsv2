page_title: Using Environment Variables for Continuous Integration
page_description: How to use Environment variables and customize them for your Build Configuration
page_keywords: continuous integration environment variables secure encrypted custom travis ci compatibility


# Using environment variables
You can customize your CI workflow by using environment variables. These variables are available to you in all sections of your yml so you can use conditional statements depending on the value(s) of one or more variables.

Shippable provides a standard list of environment variables that are available for every build. You can also define your own variables and use them for your CI workflow. 


## Standard variables

Standard variables are pre-populated by Shippable and are available to you for every build. A complete list is provided below:


| Env variable        | Description           |
| ------------- |-------------|
|BASE_BRANCH		 | Name of the target branch into which the pull request changes will be merged.|
|BRANCH		 | Name of branch being built.|
|BUILD_NUMBER		 | Build number for current build.|
|BUILD_URL		 | Direct URL link to the build output.|
|CACHE_CONTAINER    | false |
|CACHE_DIR    |  If cache is true in the build section of `shippable.yml`, then **true**.  Otherwise **false**.  |
|CI		 | true|
|COMMIT  |Commit id that is being built and tested. |
|COMMITTER  |Name of the last committer. |
|COMMIT_MESSAGE  |Commit message specified in the commit. |
|COMPARE_URL		 |A link to GitHub/Bitbucket's comparision view for the push. |
|CONTINUOUS_INTEGRATION	 |true |
|DEBIAN_FRONTEND		 |noninteractive |
|HEAD_BRANCH		 | This is only set for pull requests and is the name of the branch the pull request was opened from.|
|IS_PULL_REQUEST     |Set to **true** if the job is a pull request. If not, this will be set to **false**. |
|IS_FORK    |Set to **true** if the job belongs to a forked project. If not, this will be set to **false**. |
|JOB_ID		 | ID of job in Shippable.|
|JOB_NUMBER, SHIPPABLE_JOB_NUMBER, SHIPPABLE_JOB_ID | All three variables are the same & represent the number of the job in Shippable.|
|LANG		 |en_US.UTF-8 |
|LAST_AUTHOR |Name of the last author. |
|LAST_SUCCESSFUL_BUILD_TIMESTAMP		 |Timestamp of the last successful build in seconds. This will be set to **false** for the first build or for the build with no prior successful builds. |
|LC_ALL 		 |en_US.UTF-8 |
|LC_CTYPE 		 | en_US.UTF-8|
|MERB_ENV 		 |test |
|ORG_NAME     | Name of the organization/user that owns the repository currently being built (eg. This will be set to `Shippable` if the full name is `Shippable/support`).|
|PATH		 | $HOME/bin:$PATH:$HOME/usr/local/bin|
|PROJECT_ID | ID of the Shippable Project. |
|PULL_REQUEST		 |Pull request number if the job is a pull request. If not, this will be set to **false**. |
|PULL_REQUEST_BASE_BRANCH | Name of the branch that the pull request will be merged into. It should be the same as BASE_BRANCH.|
|RACK_ENV 		 | test|
|RAILS_ENV		 |test |
|REPO_FULL_NAME     | Full name of the repository currently being built (eg. `Shippable/support`).|
|REPO_NAME 		 | Name of the repository currently being built (eg. This will be set to `support` if the full name is `Shippable/support`).|
|REPOSITORY_URL 		 |URL of your GitHub or Bitbucket repository. |
|SERVICE_SKIP 		 |false |
|SHIPPABLE		 | true|
|SHIPPABLE_ARCHIVE 		 | true|
|SHIPPABLE_BUILD_DIR | Directory where the repository is cloned. |
|SHIPPABLE_BUILD_ID 		 |ID of build in Shippable. |
|SHIPPABLE_BUILD_NUMBER | Build number for current build. |
|SHIPPABLE_BUNDLER_ARGS  | The value of bundler_args in the build section of `shippable.yml`. |
|SHIPPABLE_COMMIT_RANGE  | Parent commit… current commit being built.  |
|SHIPPABLE_DATA_DIR | $HOME/data|
|SHIPPABLE_GEMFILE | The gemfile specified for the job in the `shippable.yml`. |
| SHIPPABLE_GO_VERSION | GO version specified for the job. |
| SHIPPABLE_GOPATH | PATH set to $HOME |
|SHIPPABLE_JDK_VERSION | The jdk for the job in the `shippable.yml`. |
|SHIPPABLE_LEIN_VERSION		 | Clojure version specified for the job.|
|SHIPPABLE_MYSQL_BINARY		 |"/usr/bin/mysqld_safe" |
|SHIPPABLE_MYSQL_CMD		 |"\$SHIPPABLE_MYSQL_BINARY" |
| SHIPPABLE_NODE_VERSION | NodeJS version specified for the job. |
|SHIPPABLE_OS_NAME | linux|
| SHIPPABLE_PHP_VERSION | PHP version specified for the job. |
|SHIPPABLE_POSTGRES_VERSION		 | "9.2"|
|SHIPPABLE_POSTGRES_BINARY		 |"/usr/lib/postgresql/\$SHIPPABLE_POSTGRES_VERSION/bin/postgres" |
|SHIPPABLE_POSTGRES_CMD 		 | "sudo -u postgres \$SHIPPABLE_POSTGRES_BINARY -c "config_file=/etc/postgresql/\$SHIPPABLE_POSTGRES_VERSION/main/postgresql.conf""|
| SHIPPABLE_PYTHON_VERSION | Python version specified for the job. |
|SHIPPABLE_REPO_DIR | The directory where builds run. |
|SHIPPABLE_REPO_SLUG | Full name of the repository being built (e.g. `Shippable/support`). |
| SHIPPABLE_RUBY | Ruby version version specified for the job. |
| SHIPPABLE_SCALA_VERSION | Scala version specified for the job. |
|SHIPPABLE_SELENIUM_PORT | 4444|
|SHIPPABLE_SELENIUM_BINARY |Location of selenium binary. It is set only if selenium is in the addons or services in the `shippable.yml`. |
|SHIPPABLE_SUBMODULE_ENABLED | Whether or not submodules in the repository will be updated. |
|SHIPPABLE_VE_DIR		 | "\$HOME/build_ve/python/2.7"|
|SUBSCRIPTION_ID | ID of the Subscription. |
|IS_GIT_TAG | Set to **true** if the build is triggered by a git tag push webhook. If not, this will be set to **false**. This env variable is currently supported for GitHub only.|
|GIT_TAG_NAME | The git tag name if the build is triggered by a git tag push webhook or a release webhook. This env variable is currently supported for GitHub only.|
|IS_RELEASE | Set to **true** if the build is triggered by a release webhook. If not, this will be set to **false**. This env variable is currently supported for GitHub only.|
|IS_PRERELEASE | Set to **true** if the release is marked pre-release when it was published. If not, this will be set to **false**. This env variable is currently supported for GitHub only.|
|RELEASED_AT | The timestamp when the release was published. This env variable is currently supported for GitHub only.|
|RELEASE_NAME | The name of the release webhook. This env variable is currently supported for GitHub only.|



### Travis compatible variables
We support several environment variables to help customers migrating from Travis CI. These are listed below:

| Env variable        | Description           |
| ------------- |-------------|
| TRAVIS		 | true|
| TRAVIS_OS_NAME | linux|
| TRAVIS_BUILD_DIR | |
| TRAVIS_REPO_SLUG | |
| TRAVIS_COMMIT_RANGE | |
| TRAVIS_BUILD_NUMBER | |
| TRAVIS_JOB_NUMBER | |
| TRAVIS_BUILD_ID | |
| TRAVIS_JOB_ID | |
| TRAVIS_BRANCH | |
| TRAVIS_COMMIT | |
| TRAVIS_PULL_REQUEST | |
| TRAVIS_NODE_VERSION | |
| TRAVIS_PHP_VERSION | |
| TRAVIS_PYTHON_VERSION | |
| TRAVIS_RUBY_VERSION | |
| TRAVIS_SCALA_VERSION | |

---

## User defined Variables

If you need your own environment variables for your build, you can specify these in your yml:

```
# environment variable
env:
  - FOO=foo BAR=bar
```

If you want to trigger builds against combinations of environment variables, you can include the combinations you need, one on each line. FOr example, the yml below will trigger 2 builds for each commit:

```
# environment variable
env:
  - TEST=true PROD=false
  - TEST=false PROD=true
```

Env variables can create an exponential number of builds when combined with `jdk` & `rvm , node_js etc.` i.e. it is multiplicative.

For an example, please check out the [Matrix Builds section](/ci/advancedOptions/matrixBuilds/).

To avoid a matrix build and kick off a single build with all environments, you can use the global tag as detailed in the section below.

###Global variables
If you want to define one or more environment variables that do not trigger multiple builds, you should use the `global` tag.

```
env:
  global:
    - FOO=true
    - BAR=false
```
The snippet above will trigger just one build with both variables set.

If you want to define a set of variables that are global, and another set that trigger a build matrix, you can configure that as shown below:

```
env:
  global:
    - FOO=true
    - BAR=false
  matrix:
    - TEST=test
    - PROD=prod

```
The snippet above will trigger 2 builds: one with environment variables (TEST=test, FOO=true, BAR=false) and another with (PROD=prod, FOO=true, BAR=false).

You can also use encrypted variables in the global or matrix sections if needed. More on encrypted variables below.

### Secure variables
If you need to include environment variables that are used to store sensitive information like passwords or keys, you should encrypt them and enter the encrypted value in your yml.

To encrypt an environment variable, go to your Shippable subscription or project settings. Choose 'Encrypt' from the left sidebar menu. Enter your variables in the textbox in the Encrypt section and click on `Encrypt`. This will give you the encrypted string that you can use in your yml. A more detailed [explanation is given here](../../navigatingUI/projects/settings.md#Encrypt)

<img src="../../images/advancedOptions/encrypt.png" alt="Encrypt Environment Variables" style="width:1000px;"/>

You can then include the encrypted string in your yml using the `secure` tag. The variables are then available during your build:

```
env:
  - secure: <encrypted output>

build:
  ci:
    - echo $FOO  
``` 

As with other user defined environment variables, you can specify multiple secure variables in your yml to trigger a build matrix or specify them as global. 

**Things to remember**

* Due to the security risk of exposing your secure variables, we do not decrypt secure variables for pull request from the forks of public projects. Secure variable decryption is limited to the pull request triggered from the branches on the same repository. 
* Decrypted secured variables are not displayed in the script tab for security reasons.
* Project owners can decrypt secure variables by going to their Settings tab, choosing Encrypt in the sidebar menu, and then entering the secure variable in the Decrypt section. More on [this is explained here](../../navigatingUI/projects/settings.md#Encrypt).

