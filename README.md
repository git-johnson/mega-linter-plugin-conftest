# MegaLinter Conftest Plugin

A MegaLinter plugin that integrates [Conftest](https://www.conftest.dev/) for policy testing across various configuration file formats.

## Description

This plugin enables MegaLinter to use Conftest to test your configuration files against policies written in Rego. Conftest allows you to write tests against structured configuration data using the Open Policy Agent (OPA) Rego policy language.

## Supported File Formats

The plugin supports linting and policy validation for the following file formats:

- CUE (.cue)
- CycloneDX (.cdx.json, .cdx.xml)
- Dockerfile (Dockerfile, .dockerfile)
- EDN (.edn)
- Environment files (.env)
- HCL (.tf, .hcl, .hcl2)
- HOCON (.conf)
- Ignore files (.gitignore, .dockerignore)
- INI (.ini)
- JSON (.json)
- Jsonnet (.jsonnet, .libsonnet)
- Property files (.properties)
- SPDX (.spdx, .spdx.json)
- TextProto (.textproto)
- TOML (.toml)
- VCL (.vcl)
- XML (.xml)
- YAML (.yaml, .yml)

## Installation

### Using with MegaLinter

Add this plugin to your MegaLinter configuration:

```yaml
PLUGINS:
  - "https://raw.githubusercontent.com/git-johnson/mega-linter-plugin-conftest/main/conftest.megalinter-descriptor.yml"
```

### Prerequisites

- MegaLinter installed and configured
- Docker if using the containerized version of MegaLinter

## Usage

### Basic Usage

By default, the plugin will look for Rego policies in the `policy` directory of your repository.

### Configuration

You can customize the plugin behavior by configuring the following environment variables in your MegaLinter configuration:

```yaml
CONFTEST_CONFTEST_ARGUMENTS: "--additional-args"  # Additional CLI arguments for Conftest
CONFTEST_CONFTEST_FILTER_REGEX_INCLUDE: ""  # Include only certain files
CONFTEST_CONFTEST_FILTER_REGEX_EXCLUDE: ""  # Exclude specific files
CONFTEST_CONFTEST_FILE_EXTENSIONS: # Only target certain file extensions
  - ".tf"
```

### Writing Policies

Create a `policy` directory (or the directory specified in your configuration) and add Rego policy files with the `.rego` extension.

Example policy (`policy/deny.rego`):

```rego
package main

deny[msg] {
  input.kind == "Deployment"
  not input.spec.template.spec.securityContext.runAsNonRoot

  msg = "Containers must not run as root"
}
```

### Examples

Testing YAML files with default policy path:

```bash
# MegaLinter will run:
conftest test path/to/your/file.yaml
```

## Troubleshooting

- Ensure your policy files are correctly formatted and located in the configured policy directory
- Check that your configuration files are valid for their respective formats
- For debugging, enable more verbose output with `MEGALINTER_LOG_LEVEL: DEBUG`

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the AGPL-3.0 license

## Resources

- [Conftest Documentation](https://www.conftest.dev/)
- [Open Policy Agent](https://www.openpolicyagent.org/)
- [MegaLinter Documentation](https://megalinter.io/)
