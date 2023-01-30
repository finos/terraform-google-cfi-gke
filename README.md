# Terraform Child Module Template

## To get a new child module

To clone this repo for a new child module, please create an issue using the following format:

- **Title:**  `Create repo: your-repo-name`
- **Subject:** *A short blurb explaining what this repo is for and who will contribute to it*

Discussion from the community and approval from the maintainers should then take place. Upon maintainer approval, a FINOS technical support staff member should be contacted and assigned the issue.

## After your new repo is created

Adjust this readme as part of your first commit after creating a repository from this template.

[See here](https://github.com/terraform-docs/terraform-docs) for more information about the Terraform docs generator.

Example of generated docs is below.

-----

<!-- BEGIN_TF_DOCS -->
[![FINOS - Incubating](https://cdn.jsdelivr.net/gh/finos/contrib-toolbox@master/images/badge-incubating.svg)](https://finosfoundation.atlassian.net/wiki/display/FINOS/Incubating)
![website build](https://github.com/finos/cfi-terraform-template-child-module/workflows/Docusaurus-website-build/badge.svg)

# terraform-provider-function

This terraform module produces blah

## Usage example
```hcl
module "iam" {
  source  = "terraform-aws-modules/iam/aws"
  version = "5.3.0"
}
```

### Providers

No providers.

### Requirements

No requirements.

### Inputs

No inputs.

### Outputs

No outputs.

### Resources

No resources.

## Installation

OS X & Linux:

```sh
npm install my-crazy-module --save
```

Windows:

```sh
edit autoexec.bat
```
## Development setup

Describe how to install all development dependencies and how to run an automated test-suite of some kind. Potentially do this for multiple platforms.

```sh
make install
npm test
```
## Roadmap

List the roadmap steps; alternatively link the Confluence Wiki page where the project roadmap is published.

1. Item 1
2. Item 2
3. ....

## Contributing

1. Fork it (<https://github.com/finos/cfi-terraform-template-child-module/fork>)
2. Create your feature branch (`git checkout -b feature/fooBar`)
3. Read our [contribution guidelines](.github/CONTRIBUTING.md) and [Community Code of Conduct](https://www.finos.org/code-of-conduct)
4. Commit your changes (`git commit -am 'Add some fooBar'`)
5. Push to the branch (`git push origin feature/fooBar`)
6. Create a new Pull Request

\_NOTE:\_ Commits and pull requests to FINOS repositories will only be accepted from those contributors with an active, executed Individual Contributor License Agreement (ICLA) with FINOS OR who are covered under an existing and active Corporate Contribution License Agreement (CCLA) executed with FINOS. Commits from individuals not covered under an ICLA or CCLA will be flagged and blocked by the FINOS Clabot tool (or [EasyCLA](https://community.finos.org/docs/governance/Software-Projects/easycla)). Please note that some CCLAs require individuals/employees to be explicitly named on the CCLA.

*Need an ICLA? Unsure if you are covered under an existing CCLA? Email [help@finos.org](mailto:help@finos.org)*

## License

Copyright 2022 FINOS

Distributed under the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).

SPDX-License-Identifier: [Apache-2.0](https://spdx.org/licenses/Apache-2.0)
<!-- END_TF_DOCS -->

Run this when you create a repo: 

```sh
echo '/usr/local/bin/terraform-docs md --output-file README.md .' >> .git/hooks/pre-commit && chmod a+x .git/hooks/pre-commit
```

Then delete this message.
