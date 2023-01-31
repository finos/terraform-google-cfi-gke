# Contribution and Governance Policies

Please review the CFI project [contribution guidelines](https://github.com/finos/compliant-financial-infrastructure/blob/dev/.github/CONTRIBUTING.md) for details about the contribution proocess and workflows.

## Dependencies

The following dependencies must be installed on the development system:

- [Terraform](https://www.terraform.io/downloads)
- [Terraform-docs](https://terraform-docs.io/user-guide/installation/)

## Generating Documentation for Inputs and Outputs

The Inputs and Outputs tables in the READMEs of the root module,
submodules, and example modules are automatically generated based on
the `variables` and `outputs` of the respective modules. These tables
must be refreshed if the module interfaces are changed.

### Autogeneration of documentation from .tf files
The README will be generated during a pull-request where github-actions for
`terraform-docs`.

## Integration Testing

Integration tests are used to verify the behaviour of the root module,
submodules, and example modules. Additions, changes, and fixes should
be accompanied with tests.

The integration tests are run using Terraform's [native]
(https://www.terraform.io/language/modules/testing-experiment) module testing
feature. The command to run locally would be `terraform test`. Tests would be in
the following format:

```
$ terraform test
─── Failed: defaults.api_url.scheme (default scheme is https) ───────────────
wrong value
    got:  "http"
    want: "https"
─────────────────────────────────────────────────────────────────────────────
```

The general strategy for these tests is to verify the behaviour of the
[defaults](./tests/defaults/), thus ensuring that the root module,
submodules, and example modules are all functionally correct.

### Test Environment
The easiest way to test the module is in an isolated test project. The
setup for such a project is defined in [tests/setup](./tests/setup/)
directory.

To use this setup, you need a (fill-in-blanks)

### Noninteractive Execution

TODO: Describe Github-Actions for Terraform.

### Interactive Execution

1. Run `terraform test` to start the testing of the module.

## Linting and Formatting

Many of the files in the repository can be linted or formatted to
maintain a standard of quality.
