---
# ----------------------------------------------------------------------------
#
#     ***     AUTO GENERATED CODE    ***    Type: MMv1     ***
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
subcategory: "Apigee"
description: |-
  An `Environment` in Apigee.
---

# google\_apigee\_environment

An `Environment` in Apigee.


To get more information about Environment, see:

* [API documentation](https://cloud.google.com/apigee/docs/reference/apis/apigee/rest/v1/organizations.environments/create)
* How-to Guides
    * [Creating an environment](https://cloud.google.com/apigee/docs/api-platform/get-started/create-environment)

## Example Usage - Apigee Environment Basic


```hcl
data "google_client_config" "current" {}

resource "google_compute_network" "apigee_network" {
  name = "apigee-network"
}

resource "google_compute_global_address" "apigee_range" {
  name          = "apigee-range"
  purpose       = "VPC_PEERING"
  address_type  = "INTERNAL"
  prefix_length = 16
  network       = google_compute_network.apigee_network.id
}

resource "google_service_networking_connection" "apigee_vpc_connection" {
  network                 = google_compute_network.apigee_network.id
  service                 = "servicenetworking.googleapis.com"
  reserved_peering_ranges = [google_compute_global_address.apigee_range.name]
}

resource "google_apigee_organization" "apigee_org" {
  analytics_region   = "us-central1"
  project_id         = data.google_client_config.current.project
  authorized_network = google_compute_network.apigee_network.id
  depends_on         = [google_service_networking_connection.apigee_vpc_connection]
}

resource "google_apigee_environment" "env" {
  name         = "tf-test%{random_suffix}"
  description  = "Apigee Environment"
  display_name = "environment-1"
  org_id       = google_apigee_organization.apigee_org.id
}
```

## Argument Reference

The following arguments are supported:


* `name` -
  (Required)
  The resource ID of the environment.

* `org_id` -
  (Required)
  The Apigee Organization associated with the Apigee environment,
  in the format `organizations/{{org_name}}`.


- - -


* `display_name` -
  (Optional)
  Display name of the environment.

* `description` -
  (Optional)
  Description of the environment.

* `deployment_type` -
  (Optional)
  Optional. Deployment type supported by the environment. The deployment type can be
  set when creating the environment and cannot be changed. When you enable archive
  deployment, you will be prevented from performing a subset of actions within the
  environment, including:
  Managing the deployment of API proxy or shared flow revisions;
  Creating, updating, or deleting resource files;
  Creating, updating, or deleting target servers.
  Possible values are `DEPLOYMENT_TYPE_UNSPECIFIED`, `PROXY`, and `ARCHIVE`.

* `api_proxy_type` -
  (Optional)
  Optional. API Proxy type supported by the environment. The type can be set when creating
  the Environment and cannot be changed.
  Possible values are `API_PROXY_TYPE_UNSPECIFIED`, `PROGRAMMABLE`, and `CONFIGURABLE`.

* `node_config` -
  (Optional)
  NodeConfig for setting the min/max number of nodes associated with the environment.
  Structure is [documented below](#nested_node_config).


<a name="nested_node_config"></a>The `node_config` block supports:

* `min_node_count` -
  (Optional)
  The minimum total number of gateway nodes that the is reserved for all instances that
  has the specified environment. If not specified, the default is determined by the
  recommended minimum number of nodes for that gateway.

* `max_node_count` -
  (Optional)
  The maximum total number of gateway nodes that the is reserved for all instances that
  has the specified environment. If not specified, the default is determined by the
  recommended maximum number of nodes for that gateway.

* `current_aggregate_node_count` -
  The current total number of gateway nodes that each environment currently has across
  all instances.

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are exported:

* `id` - an identifier for the resource with format `{{org_id}}/environments/{{name}}`


## Timeouts

This resource provides the following
[Timeouts](https://developer.hashicorp.com/terraform/plugin/sdkv2/resources/retries-and-customizable-timeouts) configuration options:

- `create` - Default is 30 minutes.
- `update` - Default is 20 minutes.
- `delete` - Default is 30 minutes.

## Import


Environment can be imported using any of these accepted formats:

```
$ terraform import google_apigee_environment.default {{org_id}}/environments/{{name}}
$ terraform import google_apigee_environment.default {{org_id}}/{{name}}
```
