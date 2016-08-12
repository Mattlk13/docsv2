page_title: Using Caching in your Continuous Integration/Continuous Delivery projects
page_description: How to leverage caching in your Build Configuration
page_keywords: getting started, questions, documentation, shippable, config, yml, cache, container, Docker


# Caching

Caching can have a dramatic impact in build durations since it provides a way to preserve dependencies and packages between builds so you don't spend the time installing them for each build. If caching is turned on, it is updated for every build and is available to subsequent builds.

For a deeper look at how caching works, you can read the [Caching mechanics section.](#cacheMechanics) 

##Turning on cache
You can turn on caching for your builds by including `cache: true` in the `build` section of your shippable.yml. This will cache contents of the build directory $SHIPPABLE_BUILD_DIR.

```
build:
  cache: true
```

You can also choose to cache specific folders instead of the entire build directory by using the `cache_dir_list` tag.

The cache_dir_list is an array of **absolute path** of the folders that needs to be cached. Please note that you still need the `cache: true` in your yml:

```
build:
  cache: true
  cache_dir_list:
    - absolute path of dir1
    - absolute path of dir2
    - absolute path of dir3
```

###Caching node modules

To cache node modules, use the snippet below:

```
build:
  cache: true
  cache_dir_list:
    - $SHIPPABLE_BUILD_DIR/node_modules
```

###Caching ruby gems

The `bundle install` command can take a while depending on what needs to be installed. To avoid paying this installation tax for each build, you can choose to cache your gems using the yml snippet below as a guide:

```
build:
  ci:
    - bundle package --all
    - bundle install --gemfile="Gemfile"
  cache: true
  cache_dir_list: 
    # replace 2.2.2 with the ruby version you need to cache
    - /usr/local/rvm/gems/$(rvm strings 2.2.2)/gems
    - /usr/local/rvm/gems/$(rvm strings 2.2.2)/specifications
    - $SHIPPABLE_BUILD_DIR
   
```
If the above directories don't work for you, you should check where the gems are installed in your container. To do this, cd /usr/local/rvm/gems and see which directories are in there and need to be cached.

---

## Clearing cache
You can clear cache in one of two ways:

* Including [reset minion] or [reset_minion] in your commit message.
* Clicking on the `Clear cache` in your Project settings UI.

In both cases, your cached content will be deleted. This is a one time action, so if cache is still set to true in your yml, the build will generate a new cache which will be used for subsequent builds.

This method is the best way to update your cache, if required.

---

## Removing unwanted files when caching is enabled
When caching is enabled, the entire build directory including artifacts (when `cache: true` is used) or the entire folder contents (if configured for specific folders) is cached.

This may include unwanted files.

Unless these unwanted files are removed, your tests could fail during the CI build, as Shippable updates the cached directory with the new commits.

Remove the unwanted files in one of the two ways:

* Use the `git clean` command: In your `shippable.yml` add the `git clean` in the `ci` section using the following format:
    - `git clean -f` removes all untracked files not ignored by `.gitignore`.
    - `git clean -f -X` removes all untracked files included in `.gitignore` only.
    - `git clean -f -x` removes all untracked files.

* Add commands such as `rm` to remove the unwanted files in your `shippable.yml` in the `ci` section.  

---

##<a name="cacheMechanics"></a>Caching mechanics

If you set cache to true, all specified folders are added to a tar file and uploaded to S3 at the end of your build. The URL where the tar file is uploaded is completely secure and unique to your cache.

When the next build is triggered, we check if cache exists. If yes, we will download the tar file and unzip it, so that the contents are available during your build. 

The rountrip from S3 means that caching is very useful in specific scenarios and not in others:

* Files that take a long time to install benefit from caching. So anything related to bundler, npm, composer, pip, etc are great candidates for caching.
* Files that take a long time to download but installed quickly do not benefit from caching since it takes as much time to download from S3 as from the original source. Examples are compiled binaries, JDK packages, etc. 

Caching per branch is currently unsupported, so your cache will be updated for each build regardless of branch. Also, if you're running multiple builds per trigger, aka matrix builds, the cache will be updated for each build in the matrix and available to the next.

