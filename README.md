# Launchable record build and test results action

[Launchable](https://www.launchableinc.com/) is a software development intelligence platform currently focused on continuous integration (CI). Using data from your CI runs, Launchable provides various features to speed up your testing workflow so you can ship high quality software faster.

Launchable `record-build-and-test-results-action` enables you to integrate Launchable into your CI in simple way with less change. This action installs [launchableinc/cli](https://github.com/launchableinc/cli) and runs `launchable record build` and `launchable record test` to send data to Launchable so that the test results will be analyzed in [Launchable](https://www.launchableinc.com/) to improve your developer productivity. You still need to add [subset request command](https://docs.launchableinc.com/resources/cli-reference#subset) to retrieve test subset.

# Usage

```yaml
name: Test

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  LAUNCHABLE_DEBUG: 1
  LAUNCHABLE_REPORT_ERROR: 1

jobs:
  tests:
    env:
      LAUNCHABLE_TOKEN: ${{ secrets.LAUNCHABLE_TOKEN }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Test
        run: <YOUR TEST COMMAND HERE>
      - name: Record
        uses: launchableinc/record-build-and-test-results-action@v1.0.0
        with:
          build_name: $GITHUB_RUN_ID
          report_path: .
          test_runner: <YOUR TEST RUNNER HERE>
        if: always()
```

## Example

Refer [go-test example](./.github/workflows/go-test-example.yaml) for example use of the action.

# Inputs

### `test_runner`
**Required**

The name of your test runner. Our supported test runners are [here](https://docs.launchableinc.com/resources/integrations).

### `report_path`
**Required**

Path to the test report generated by your test runner. Refer [here](https://docs.launchableinc.com/resources/integrations) for how to generate a test report in your test runner.

### `source_path`
**Optional**

Path to a local Git repository/workspace. Default `.`.

### `build_name`
**Optional**

[Build](https://docs.launchableinc.com/concepts/build) name that you can give to the current software. Default `GitHub SHA`.

### `max_days`
**Optional**

The maximum number of days to collect commits retroactively. Default `30` days.

### `no_submodules`
**Optional**

Flag to stop collecting build information from Git Submodules. Default `false`, which means the submodules are collected.

### `python_version`
**Optional**

Python version to use for the Launchable CLI. Default `3.10`. Change this if your test running environment requires a specific Python version.

# License
Launchable data collection action is licensed under [Apache license](./LICENSE).
