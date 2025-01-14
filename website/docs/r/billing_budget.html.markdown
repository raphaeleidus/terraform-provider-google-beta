---
# ----------------------------------------------------------------------------
#
#     ***     AUTO GENERATED CODE    ***    AUTO GENERATED CODE     ***
#
# ----------------------------------------------------------------------------
#
#     This file is automatically generated by Magic Modules and manual
#     changes will be clobbered when the file is regenerated.
#
#     Please read more about how to change this file in
#     .github/CONTRIBUTING.md.
#
# ----------------------------------------------------------------------------
subcategory: "Billing Budget"
layout: "google"
page_title: "Google: google_billing_budget"
sidebar_current: "docs-google-billing-budget"
description: |-
  Budget configuration for a billing account.
---

# google\_billing\_budget

Budget configuration for a billing account.

~> **Warning:** This resource is in beta, and should be used with the terraform-provider-google-beta provider.
See [Provider Versions](https://terraform.io/docs/providers/google/guides/provider_versions.html) for more details on beta resources.

To get more information about Budget, see:

* [API documentation](https://cloud.google.com/billing/docs/reference/budget/rest/v1beta1/billingAccounts.budgets)
* How-to Guides
    * [Creating a budget](https://cloud.google.com/billing/docs/how-to/budgets)

<div class = "oics-button" style="float: right; margin: 0 0 -15px">
  <a href="https://console.cloud.google.com/cloudshell/open?cloudshell_git_repo=https%3A%2F%2Fgithub.com%2Fterraform-google-modules%2Fdocs-examples.git&cloudshell_working_dir=billing_budget_basic&cloudshell_image=gcr.io%2Fgraphite-cloud-shell-images%2Fterraform%3Alatest&open_in_editor=main.tf&cloudshell_print=.%2Fmotd&cloudshell_tutorial=.%2Ftutorial.md" target="_blank">
    <img alt="Open in Cloud Shell" src="//gstatic.com/cloudssh/images/open-btn.svg" style="max-height: 44px; margin: 32px auto; max-width: 100%;">
  </a>
</div>
## Example Usage - Billing Budget Basic


```hcl
data "google_billing_account" "account" {
  provider = google-beta
  billing_account = "000000-0000000-0000000-000000"
}

resource "google_billing_budget" "budget" {
  provider = google-beta
  billing_account = data.google_billing_account.account.id
  display_name = "Example Billing Budget"
  amount {
    specified_amount {
      currency_code = "USD"
      units = "100000"
    }
  }
  threshold_rules {
      threshold_percent =  0.5
  }
}
```
<div class = "oics-button" style="float: right; margin: 0 0 -15px">
  <a href="https://console.cloud.google.com/cloudshell/open?cloudshell_git_repo=https%3A%2F%2Fgithub.com%2Fterraform-google-modules%2Fdocs-examples.git&cloudshell_working_dir=billing_budget_filter&cloudshell_image=gcr.io%2Fgraphite-cloud-shell-images%2Fterraform%3Alatest&open_in_editor=main.tf&cloudshell_print=.%2Fmotd&cloudshell_tutorial=.%2Ftutorial.md" target="_blank">
    <img alt="Open in Cloud Shell" src="//gstatic.com/cloudssh/images/open-btn.svg" style="max-height: 44px; margin: 32px auto; max-width: 100%;">
  </a>
</div>
## Example Usage - Billing Budget Filter


```hcl
data "google_billing_account" "account" {
  provider = google-beta
  billing_account = "000000-0000000-0000000-000000"
}

resource "google_billing_budget" "budget" {
  provider = google-beta
  billing_account = data.google_billing_account.account.id
  display_name = "Example Billing Budget"

  budget_filter {
    projects = ["projects/example-project"]
    credit_types_treatment = "EXCLUDE_ALL_CREDITS"
    services = ["services/24E6-581D-38E5"] # Bigquery
  }

  amount {
    specified_amount {
      currency_code = "USD"
      units = "100000"
    }
  }

  threshold_rules {
    threshold_percent = 0.5
  }
  threshold_rules {
    threshold_percent = 0.9
    spend_basis = "FORECASTED_SPEND"
  }
}
```

## Argument Reference

The following arguments are supported:


* `amount` -
  (Required)
  The budgeted amount for each usage period.  Structure is documented below.

* `threshold_rules` -
  (Required)
  Rules that trigger alerts (notifications of thresholds being
  crossed) when spend exceeds the specified percentages of the
  budget.  Structure is documented below.

* `billing_account` -
  (Required)
  ID of the billing account to set a budget on.



The `budget_filter` block supports:

* `projects` -
  (Optional)
  A set of projects of the form projects/{project_id},
  specifying that usage from only this set of projects should be
  included in the budget. If omitted, the report will include
  all usage for the billing account, regardless of which project
  the usage occurred on. Only zero or one project can be
  specified currently.

* `credit_types_treatment` -
  (Optional)
  Specifies how credits should be treated when determining spend
  for threshold calculations.

* `services` -
  (Optional)
  A set of services of the form services/{service_id},
  specifying that usage from only this set of services should be
  included in the budget. If omitted, the report will include
  usage for all the services. The service names are available
  through the Catalog API:
  https://cloud.google.com/billing/v1/how-tos/catalog-api.

The `amount` block supports:

* `specified_amount` -
  (Required)
  A specified amount to use as the budget. currencyCode is
  optional. If specified, it must match the currency of the
  billing account. The currencyCode is provided on output.  Structure is documented below.


The `specified_amount` block supports:

* `currency_code` -
  (Optional)
  The 3-letter currency code defined in ISO 4217.

* `units` -
  (Optional)
  The whole units of the amount. For example if currencyCode
  is "USD", then 1 unit is one US dollar.

* `nanos` -
  (Optional)
  Number of nano (10^-9) units of the amount.
  The value must be between -999,999,999 and +999,999,999
  inclusive. If units is positive, nanos must be positive or
  zero. If units is zero, nanos can be positive, zero, or
  negative. If units is negative, nanos must be negative or
  zero. For example $-1.75 is represented as units=-1 and
  nanos=-750,000,000.

The `threshold_rules` block supports:

* `threshold_percent` -
  (Required)
  Send an alert when this threshold is exceeded. This is a
  1.0-based percentage, so 0.5 = 50%. Must be >= 0.

* `spend_basis` -
  (Optional)
  The type of basis used to determine if spend has passed
  the threshold.

The `all_updates_rule` block supports:

* `pubsub_topic` -
  (Required)
  The name of the Cloud Pub/Sub topic where budget related
  messages will be published, in the form
  projects/{project_id}/topics/{topic_id}. Updates are sent
  at regular intervals to the topic.

* `schema_version` -
  (Optional)
  The schema version of the notification. Only "1.0" is
  accepted. It represents the JSON schema as defined in
  https://cloud.google.com/billing/docs/how-to/budgets#notification_format.

- - -


* `display_name` -
  (Optional)
  User data for display name in UI. Must be <= 60 chars.

* `budget_filter` -
  (Optional)
  Filters that define which resources are used to compute the actual
  spend against the budget.  Structure is documented below.

* `all_updates_rule` -
  (Optional)
  Defines notifications that are sent on every update to the
  billing account's spend, regardless of the thresholds defined
  using threshold rules.  Structure is documented below.


## Attributes Reference

In addition to the arguments listed above, the following computed attributes are exported:


* `name` -
  Resource name of the budget. The resource name
  implies the scope of a budget. Values are of the form
  billingAccounts/{billingAccountId}/budgets/{budgetId}.


## Timeouts

This resource provides the following
[Timeouts](/docs/configuration/resources.html#timeouts) configuration options:

- `create` - Default is 4 minutes.
- `update` - Default is 4 minutes.
- `delete` - Default is 4 minutes.

## Import

Budget can be imported using any of these accepted formats:

```
$ terraform import -provider=google-beta google_billing_budget.default {{name}}
```

-> If you're importing a resource with beta features, make sure to include `-provider=google-beta`
as an argument so that Terraform uses the correct provider to import your resource.
