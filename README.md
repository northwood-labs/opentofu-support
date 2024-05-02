# OpenTofu support across the ecosystem

## Summary

When the OpenTofu fork was [announced](https://opentofu.org/blog/the-opentofu-fork-is-now-available/) (September 2023), I began reaching out to several notable Terraform-related projects in the community with the goal of understanding how customers like ourselves might be impacted.

The **majority** of projects expressed taking a _wait-and-see_ approach. Some were more pro-fork, while others were more anti-fork. It was also unclear at the time how things like providers, modules, the [Terraform Plugin Framework][TPF], and other bits would be impacted by licensing changes.

### Licensing

As of this writing, the _Terraform Plugin Framework_ is licensed under the MPL-2.0. This means that _Providers_ will continue to be MPL-2.0. Modules are purely userland code, so they are not impacted. As for compatibility, Providers implement a protocol. As long as Terraform and OpenTofu both support that protocol version, the providers themselves are expected to remain compatible with both projects.

### Plans

In the (many) conversations I've had with members of the OpenTofu core team, OpenTofu:

* Is a drop-in replacement for Terraform 1.5.x and earlier.
* Is willing/interested in implementing the same _major_ features as Terraform moving forward.
* Will NOT be bug-for-bug compatible with Terraform.
* Will invest in community-driven enhancements, which may result in new features not available in Terraform. (See OpenTofu 1.7.)

## Table of research

This represents the research done to understand the plans from the Terraform/OpenTofu ecosystem. As new/changed statements are discovered, this will be updated.

| ✓ | Project                  | Notes                                                                                                                                                                                                      | Sources                         |
|:-:|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|
| × | [action/setup-terraform] | Maintained by HashiCorp. This should be used for installing Terraform in GitHub Actions.                                                                                                                   |                                 |
| ✓ | [action/setup-tofu]      | Maintained by OpenTofu. This should be used for installing OpenTofu in GitHub Actions.                                                                                                                     |                                 |
| ✓ | [Checkov]                | No anticipated impact. Should continue to work.                                                                                                                                                            | [1][chk-1]                      |
| ✓ | [Infracost]              | Infracost doesn't seem to expect much/any work on their end, and they’re not opposed to OpenTofu.                                                                                                          | [1][inf-1]                      |
| ✓ | [Provider: Artifactory]  | Maintained by JFrog. Implements the protocol, so entirely dependent on that.                                                                                                                               | [1][rt-1]                       |
| ✓ | [Provider: AWS]          | Maintained by HashiCorp. Implements the protocol, so entirely dependent on that.                                                                                                                           | [1][aws-1]                      |
| ✓ | [Provider: Datadog]      | Maintained by Datadog. Implements the protocol, so entirely dependent on that.                                                                                                                             | [1][dd-1]                       |
| ✓ | [Provider: GitHub]       | Maintained by GitHub. Implements the protocol, so entirely dependent on that.                                                                                                                              | [1][gh-1]                       |
| ✓ | [Provider: New Relic]    | Maintained by New Relic. Implements the protocol, so entirely dependent on that.                                                                                                                           | [1][nr-1]                       |
| ✓ | [Provider: PagerDuty]    | Maintained by PagerDuty. Implements the protocol, so entirely dependent on that. (Never responded.)                                                                                                        | [1][pgd-1]                      |
| ✓ | [TerraCognita]           | Embeds internal Terraform libraries (MPL-2.0). Planning to migrate to forked OpenTofu libraries. There is no “support” involved like with other projects because they don’t call out to a binary.          | [1][tcg-1]                      |
| ✓ | [terraform-docs]         | Expected to continue to work, since it does not interface with the `terraform` binary directly; it just parses the HCL.                                                                                    | [1][tfd-1]                      |
| – | [Terraformer]            | No response.                                                                                                                                                                                               | [1][tfrm-1]                     |
| ✓ | [Terragrunt]             | Gruntwork is a founding member of the OpenTofu Steering Committee. OpenTofu support made available in v0.52.0. It’s possible that Terraform will be dropped in the future (no timeline for this, however). | [1][tg-1], [2][tg-2], [3][tg-3] |
| – | [Terrascan]              | No response.                                                                                                                                                                                               | [1][tsc-1]                      |
| ✓ | [Terratest]              | Gruntwork is a founding member of the OpenTofu Steering Committee. OpenTofu support made available in v0.46.0. It’s possible that Terraform will be dropped in the future (no timeline for this, however). | [1][tt-1], [2][tt-2]            |
| ✓ | [tenv]                   | Successor to [tfenv] and [tfswitch]. Supports Terraform, [Terragrunt], and OpenTofu.                                                                                                                       |                                 |
| × | [tfenv]                  | Team has elected NOT to add support for OpenTofu. Use [tenv] instead.                                                                                                                                      | [1][tfe-1]                      |
| × | [tflint]                 | Bundles Terraform source code. License changed to BUSL-1.1 with v0.51.0. There is no technical reason that we cannot continue using this to lint code. There may be legal reasons.                         | [1][tfl-1], [2][tfl-2]          |
| ✓ | [tfschema]               | Reads the protocol, so depends on compatibility of providers. There is no “support” involved like with other projects because they don’t call out to a binary. Added explicit support in v0.7.8.           | [1][tfs-1], [2][tfs-2]          |
| × | [tfsec]                  | Deprecated. Use [Trivy] instead.                                                                                                                                                                           | [1][ts-1]                       |
| × | [tfswitch]               | Appears to be abandoned.  Use [tenv] instead.                                                                                                                                                              | [1][tfsw-1]                     |
| ✓ | [Trivy]                  | Integrates with Providers for cloud services. Will likely be supported as long as its Provider is supported.                                                                                               | [1][trv-1]                      |

[action/setup-terraform]: https://github.com/marketplace/actions/hashicorp-setup-terraform
[action/setup-tofu]: https://github.com/marketplace/actions/opentofu-setup-tofu
[Checkov]: https://www.checkov.io
[Infracost]: https://www.infracost.io
[Provider: Artifactory]: https://github.com/jfrog/terraform-provider-artifactory
[Provider: AWS]: https://github.com/hashicorp/terraform-provider-aws
[Provider: Datadog]: https://github.com/DataDog/terraform-provider-datadog
[Provider: GitHub]: https://github.com/integrations/terraform-provider-github
[Provider: New Relic]: https://github.com/newrelic/terraform-provider-newrelic
[Provider: PagerDuty]: https://github.com/PagerDuty/terraform-provider-pagerduty
[tenv]: https://github.com/tofuutils/tenv
[TerraCognita]: https://github.com/cycloidio/terracognita
[terraform-docs]: https://terraform-docs.io
[Terraformer]: https://github.com/GoogleCloudPlatform/terraformer
[Terragrunt]: https://terragrunt.gruntwork.io
[Terrascan]: https://runterrascan.io
[Terratest]: https://terratest.gruntwork.io
[tfenv]: https://github.com/tfutils/tfenv
[tflint]: https://github.com/terraform-linters/tflint
[tfschema]: https://github.com/minamijoyo/tfschema
[tfsec]: https://aquasecurity.github.io/tfsec
[tfswitch]: https://tfswitch.warrensbox.com
[TPF]: https://github.com/hashicorp/terraform-plugin-framework
[Trivy]: https://aquasecurity.github.io/trivy

[aws-1]: https://github.com/hashicorp/terraform-provider-aws/issues/33224
[chk-1]: https://github.com/bridgecrewio/checkov/issues/5501
[dd-1]: https://github.com/DataDog/terraform-provider-datadog/issues/2087
[gh-1]: https://github.com/integrations/terraform-provider-github/issues/1869
[inf-1]: https://github.com/infracost/infracost/issues/2638
[nr-1]: https://github.com/newrelic/terraform-provider-newrelic/issues/2451
[pgd-1]: https://github.com/PagerDuty/terraform-provider-pagerduty/issues/740
[rt-1]: https://github.com/jfrog/terraform-provider-artifactory/issues/791
[tcg-1]: https://github.com/cycloidio/terracognita/issues/397
[tfd-1]: https://github.com/terraform-docs/terraform-docs/issues/699
[tfe-1]: https://github.com/tfutils/tfenv/issues/409
[tfl-1]: https://github.com/terraform-linters/tflint/discussions/1826
[tfl-2]: https://github.com/terraform-linters/tflint/releases/tag/v0.51.0
[tfrm-1]: https://github.com/GoogleCloudPlatform/terraformer/issues/1773
[tfs-1]: https://github.com/minamijoyo/tfschema/issues/50
[tfs-2]: https://github.com/minamijoyo/tfschema/releases/tag/v0.7.8
[tfsw-1]: https://github.com/warrensbox/terraform-switcher/issues/315
[tg-1]: https://github.com/gruntwork-io/terragrunt/issues/2690
[tg-2]: https://www.linkedin.com/feed/update/urn:li:ugcPost:7102220371797389313/
[tg-3]: https://github.com/gruntwork-io/terragrunt/releases/tag/v0.52.0
[trv-1]: https://github.com/aquasecurity/trivy/discussions/5069
[ts-1]: https://github.com/aquasecurity/tfsec/issues/2095
[tsc-1]: https://github.com/tenable/terrascan/issues/1609
[tt-1]: https://github.com/gruntwork-io/terratest/issues/1333
[tt-2]: https://github.com/gruntwork-io/terratest/releases/tag/v0.46.0
