# codeclimate-resource
Concourse resource for submitting coverage to CodeClimate

## Source Configuration
* `api_token`: *Required.* The Test Reporter ID for this repository (available in CodeClimate under repo -> Settings -> Test Coverage)

### Example
``` yaml
---
resource_types:
- name: codeclimate
  type: docker-image
  source:
    repository: lflux/codeclimate-resource
- name: upload-codeclimate
  type: codeclimate
  source:
    api_token: ((codeclimate_api_token))
```

## Behavior

### `out`: Publish test coverage report to CodeClimate

#### Parameters
* `source_path`: *Required.* The directory containing the source and coverage file.
* `coverage_file`: Path to the coverage file inside `source_path`. Defaults to `cover.out`
* `input_type`: Coverage file type supported by [test-reporter](https://github.com/codeclimate/test-reporter). Defaults to `gocov`
* `prefix`: The prefix to remove from absolute paths in coverage payloads, to make them relative to the `source_path` root. This should mimic the path in which the tests were run.

#### Example

```yaml
---
- put: upload-codeclimate
  params:
    coverage_file: cover.out
    source_path: coverage/src/github.com/example/concourse-resource
    prefix: github.com/example/concourse-resource
```

# Caveats
Currently the resource succeeds even if the `upload-coverage` step fails, due to the test reporter failing if a coverage report for a given git SHA exists already (See [codeclimate/test-reporter #192](https://github.com/codeclimate/test-reporter/issues/192))

# Authors
* Ian Delahorne <ian.delahorne@gmail.com>
* Sebastian Vidrio <svidrio@gmail.com>

# Contributing

Please raise a [Github issue](https://github.com/lflux/codeclimate-resource/issues) if you encounter problems with this resource

# License
Licensed under the Apache 2.0 License which can be found in [LICENSE](LICENSE) file