# Test and Code Coverage Reports

Shippable can show you test and code coverage results in a consumable format where you can drill down further and find out which tests failed or which sections of your code were not covered by your tests.

Your tests results data needs to be in junit format and your code coverage results need to be in cobertura format in order to see these visualizations.

## Test Results

To set up test result visualization for a repository, do the following:

- Run tests as part of your CI workflow using shippable.yml
- Make sure test results are in junit format.
- Output test results to shippable/testresults folder.

For example, here is a sample configuration for a Python project -

```yaml
build:
  ci:
    - mkdir -p shippable/testresults
    - nosetests python/sample.py --with-xunit --xunit-file=shippable/testresults/nosetests.xml
```

Examples for other languages can be found in our [Code Samples](ci_languages/).

Once you have set this up, you can view your test results in the `Test` tab on your build page.


## Code Coverage

To set up code coverage result visualization for a repository, do the following:

- Run your code coverage command(s) as part of your CI workflow using shippable.yml
- Make sure code coverage output is in cobertura xml format.
- Output code coverage output to shippable/codecoverage folder.

For example, here is a sample configuration for a Python project -

```yaml  
build:
  ci:  
    - mkdir -p shippable/codecoverage
    - coverage run --branch python/sample.py
    - coverage xml -o shippable/codecoverage/coverage.xml python/sample.py
```

Examples for other languages can be found in our [Code Samples](ci_languages/).

Once you have set this up, you can view your code coverage results in the `Code coverage` tab on your build page.

Check out our blogs on [creating visualizations of your CI test results](http://blog.shippable.com/automatically-retry-scripts-to-avoid-network-hiccups-during-ci-process) and [setting up code coverage for tests](http://blog.shippable.com/setting-up-code-coverage-visualization-for-tests-in-ci) for additional details.
* * *
