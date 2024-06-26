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
subcategory: "Data loss prevention"
description: |-
  Configuration for discovery to scan resources for profile generation.
---

# google\_data\_loss\_prevention\_discovery\_config

Configuration for discovery to scan resources for profile generation. Only one discovery configuration may exist per organization, folder, or project.


To get more information about DiscoveryConfig, see:

* [API documentation](https://cloud.google.com/dlp/docs/reference/rest/v2/projects.locations.discoveryConfigs)
* How-to Guides
    * [Schedule inspection scan](https://cloud.google.com/dlp/docs/schedule-inspection-scan)

## Example Usage - Dlp Discovery Config Basic


```hcl
resource "google_data_loss_prevention_discovery_config" "basic" {
	parent = "projects/my-project-name/locations/us"
    location = "us"
    status = "RUNNING"

    targets {
        big_query_target {
            filter {
                other_tables {}
            }
        }
    }
    inspect_templates = ["projects/%{project}/inspectTemplates/${google_data_loss_prevention_inspect_template.basic.name}"]
}

resource "google_data_loss_prevention_inspect_template" "basic" {
	parent = "projects/my-project-name"
	description = "My description"
	display_name = "display_name"

	inspect_config {
		info_types {
			name = "EMAIL_ADDRESS"
		}
    }
}
```
## Example Usage - Dlp Discovery Config Actions


```hcl
resource "google_data_loss_prevention_discovery_config" "actions" {
	parent = "projects/my-project-name/locations/us"
    location = "us"
    status = "RUNNING"

    targets {
        big_query_target {
            filter {
                other_tables {}
            }
        }
    }
    actions {
        export_data {
            profile_table {
                project_id = "project"
                dataset_id = "dataset"
                table_id = "table"
            }
        }
    }
    actions { 
        pub_sub_notification {
            topic = "projects/%{project}/topics/${google_pubsub_topic.actions.name}"
            event = "NEW_PROFILE"
            pubsub_condition {
                expressions {
                    logical_operator = "OR"
                    conditions {
                        minimum_sensitivity_score = "HIGH"
                    }
                }
            }
            detail_of_message = "TABLE_PROFILE"
        }
    }
    inspect_templates = ["projects/%{project}/inspectTemplates/${google_data_loss_prevention_inspect_template.basic.name}"] 
}

resource "google_pubsub_topic" "actions" {
    name = "fake-topic"
}

resource "google_data_loss_prevention_inspect_template" "basic" {
	parent = "projects/my-project-name"
	description = "My description"
	display_name = "display_name"

	inspect_config {
		info_types {
			name = "EMAIL_ADDRESS"
		}
    }
}
```
## Example Usage - Dlp Discovery Config Org Running


```hcl
resource "google_data_loss_prevention_discovery_config" "org_running" {
	parent = "organizations/123456789/locations/us"
    location = "us"

    targets {
        big_query_target {
            filter {
                other_tables {}
            }
        }
    }
    org_config {
        project_id = "my-project-name"
        location {
            organization_id = "123456789"
        }
    }
    inspect_templates = ["projects/%{project}/inspectTemplates/${google_data_loss_prevention_inspect_template.basic.name}"] 
    status = "RUNNING"
}

resource "google_data_loss_prevention_inspect_template" "basic" {
	parent = "projects/my-project-name"
	description = "My description"
	display_name = "display_name"

	inspect_config {
		info_types {
			name = "EMAIL_ADDRESS"
		}
    }
}
```
## Example Usage - Dlp Discovery Config Org Folder Paused


```hcl
resource "google_data_loss_prevention_discovery_config" "org_folder_paused" {
	parent = "organizations/123456789/locations/us"
    location = "us"

    targets {
        big_query_target {
            filter {
                other_tables {}
            }
        }
    }
    org_config {
        project_id = "my-project-name"
        location {
            folder_id = 123
        }
    }
    inspect_templates = ["projects/%{project}/inspectTemplates/${google_data_loss_prevention_inspect_template.basic.name}"]
    status = "PAUSED"
}

resource "google_data_loss_prevention_inspect_template" "basic" {
	parent = "projects/my-project-name"
	description = "My description"
	display_name = "display_name"

	inspect_config {
		info_types {
			name = "EMAIL_ADDRESS"
		}
    }
}
```
## Example Usage - Dlp Discovery Config Conditions Cadence


```hcl
resource "google_data_loss_prevention_discovery_config" "conditions_cadence" {
	parent = "projects/my-project-name/locations/us"
    location = "us"
    status = "RUNNING"

    targets {
        big_query_target {
            filter {
                other_tables {}
            }
            conditions {
                type_collection = "BIG_QUERY_COLLECTION_ALL_TYPES"
            }
            cadence {
                schema_modified_cadence {
                    types = ["SCHEMA_NEW_COLUMNS"]
                    frequency = "UPDATE_FREQUENCY_DAILY"
                }
                table_modified_cadence {
                    types = ["TABLE_MODIFIED_TIMESTAMP"]
                    frequency = "UPDATE_FREQUENCY_DAILY"
                }
            }
        }
    }
    inspect_templates = ["projects/%{project}/inspectTemplates/${google_data_loss_prevention_inspect_template.basic.name}"]
}

resource "google_data_loss_prevention_inspect_template" "basic" {
	parent = "projects/my-project-name"
	description = "My description"
	display_name = "display_name"

	inspect_config {
		info_types {
			name = "EMAIL_ADDRESS"
		}
    }
}
```
## Example Usage - Dlp Discovery Config Filter Regexes And Conditions


```hcl
resource "google_data_loss_prevention_discovery_config" "filter_regexes_and_conditions" {
	parent = "projects/my-project-name/locations/us"
    location = "us"
    status = "RUNNING"

    targets {
        big_query_target {
            filter {
                tables {
                    include_regexes {
                        patterns {
                            project_id_regex = ".*"
                            dataset_id_regex = ".*"
                            table_id_regex = ".*"
                        }
                    }
                }
            }
            conditions {
                created_after = "2023-10-02T15:01:23Z"
                types {
                    types = ["BIG_QUERY_TABLE_TYPE_TABLE", "BIG_QUERY_TABLE_TYPE_EXTERNAL_BIG_LAKE"]
                }
                or_conditions {
                    min_row_count = 10
                    min_age = "10800s"
                }
            }
        }
    }
    targets {
        big_query_target {
            filter {
                other_tables {}
            }
        }
    }
    inspect_templates = ["projects/%{project}/inspectTemplates/${google_data_loss_prevention_inspect_template.basic.name}"] 
}

resource "google_data_loss_prevention_inspect_template" "basic" {
	parent = "projects/my-project-name"
	description = "My description"
	display_name = "display_name"

	inspect_config {
		info_types {
			name = "EMAIL_ADDRESS"
		}
    }
}
```

## Argument Reference

The following arguments are supported:


* `parent` -
  (Required)
  The parent of the discovery config in any of the following formats:
  * `projects/{{project}}/locations/{{location}}`
  * `organizations/{{organization_id}}/locations/{{location}}`

* `location` -
  (Required)
  Location to create the discovery config in.


- - -


* `display_name` -
  (Optional)
  Display Name (max 1000 Chars)

* `org_config` -
  (Optional)
  A nested object resource
  Structure is [documented below](#nested_org_config).

* `inspect_templates` -
  (Optional)
  Detection logic for profile generation

* `actions` -
  (Optional)
  Actions to execute at the completion of scanning
  Structure is [documented below](#nested_actions).

* `targets` -
  (Optional)
  Target to match against for determining what to scan and how frequently
  Structure is [documented below](#nested_targets).

* `status` -
  (Optional)
  Required. A status for this configuration
  Possible values are: `RUNNING`, `PAUSED`.


<a name="nested_org_config"></a>The `org_config` block supports:

* `project_id` -
  (Optional)
  The project that will run the scan. The DLP service account that exists within this project must have access to all resources that are profiled, and the cloud DLP API must be enabled.

* `location` -
  (Optional)
  The data to scan folder org or project
  Structure is [documented below](#nested_location).


<a name="nested_location"></a>The `location` block supports:

* `organization_id` -
  (Optional)
  The ID of an organization to scan

* `folder_id` -
  (Optional)
  The ID for the folder within an organization to scan

<a name="nested_actions"></a>The `actions` block supports:

* `export_data` -
  (Optional)
  Export data profiles into a provided location
  Structure is [documented below](#nested_export_data).

* `pub_sub_notification` -
  (Optional)
  Publish a message into the Pub/Sub topic.
  Structure is [documented below](#nested_pub_sub_notification).


<a name="nested_export_data"></a>The `export_data` block supports:

* `profile_table` -
  (Optional)
  Store all table and column profiles in an existing table or a new table in an existing dataset. Each re-generation will result in a new row in BigQuery
  Structure is [documented below](#nested_profile_table).


<a name="nested_profile_table"></a>The `profile_table` block supports:

* `project_id` -
  (Optional)
  The Google Cloud Platform project ID of the project containing the table. If omitted, the project ID is inferred from the API call.

* `dataset_id` -
  (Optional)
  Dataset Id of the table

* `table_id` -
  (Optional)
  Name of the table

<a name="nested_pub_sub_notification"></a>The `pub_sub_notification` block supports:

* `topic` -
  (Optional)
  Cloud Pub/Sub topic to send notifications to. Format is projects/{project}/topics/{topic}.

* `event` -
  (Optional)
  The type of event that triggers a Pub/Sub. At most one PubSubNotification per EventType is permitted.
  Possible values are: `NEW_PROFILE`, `CHANGED_PROFILE`, `SCORE_INCREASED`, `ERROR_CHANGED`.

* `pubsub_condition` -
  (Optional)
  Conditions for triggering pubsub
  Structure is [documented below](#nested_pubsub_condition).

* `detail_of_message` -
  (Optional)
  How much data to include in the pub/sub message.
  Possible values are: `TABLE_PROFILE`, `RESOURCE_NAME`.


<a name="nested_pubsub_condition"></a>The `pubsub_condition` block supports:

* `expressions` -
  (Optional)
  An expression
  Structure is [documented below](#nested_expressions).


<a name="nested_expressions"></a>The `expressions` block supports:

* `logical_operator` -
  (Optional)
  The operator to apply to the collection of conditions
  Possible values are: `OR`, `AND`.

* `conditions` -
  (Optional)
  Conditions to apply to the expression
  Structure is [documented below](#nested_conditions).


<a name="nested_conditions"></a>The `conditions` block supports:

* `minimum_risk_score` -
  (Optional)
  The minimum data risk score that triggers the condition.
  Possible values are: `HIGH`, `MEDIUM_OR_HIGH`.

* `minimum_sensitivity_score` -
  (Optional)
  The minimum sensitivity level that triggers the condition.
  Possible values are: `HIGH`, `MEDIUM_OR_HIGH`.

<a name="nested_targets"></a>The `targets` block supports:

* `big_query_target` -
  (Optional)
  BigQuery target for Discovery. The first target to match a table will be the one applied.
  Structure is [documented below](#nested_big_query_target).


<a name="nested_big_query_target"></a>The `big_query_target` block supports:

* `filter` -
  (Optional)
  Required. The tables the discovery cadence applies to. The first target with a matching filter will be the one to apply to a table
  Structure is [documented below](#nested_filter).

* `conditions` -
  (Optional)
  In addition to matching the filter, these conditions must be true before a profile is generated
  Structure is [documented below](#nested_conditions).

* `cadence` -
  (Optional)
  How often and when to update profiles. New tables that match both the fiter and conditions are scanned as quickly as possible depending on system capacity.
  Structure is [documented below](#nested_cadence).

* `disabled` -
  (Optional)
  Tables that match this filter will not have profiles created.


<a name="nested_filter"></a>The `filter` block supports:

* `tables` -
  (Optional)
  A specific set of tables for this filter to apply to. A table collection must be specified in only one filter per config.
  Structure is [documented below](#nested_tables).

* `other_tables` -
  (Optional)
  Catch-all. This should always be the last filter in the list because anything above it will apply first.


<a name="nested_tables"></a>The `tables` block supports:

* `include_regexes` -
  (Optional)
  A collection of regular expressions to match a BQ table against.
  Structure is [documented below](#nested_include_regexes).


<a name="nested_include_regexes"></a>The `include_regexes` block supports:

* `patterns` -
  (Optional)
  A single BigQuery regular expression pattern to match against one or more tables, datasets, or projects that contain BigQuery tables.
  Structure is [documented below](#nested_patterns).


<a name="nested_patterns"></a>The `patterns` block supports:

* `project_id_regex` -
  (Optional)
  For organizations, if unset, will match all projects. Has no effect for data profile configurations created within a project.

* `dataset_id_regex` -
  (Optional)
  if unset, this property matches all datasets

* `table_id_regex` -
  (Optional)
  if unset, this property matches all tables

<a name="nested_conditions"></a>The `conditions` block supports:

* `created_after` -
  (Optional)
  A timestamp in RFC3339 UTC "Zulu" format with nanosecond resolution and upto nine fractional digits.

* `or_conditions` -
  (Optional)
  At least one of the conditions must be true for a table to be scanned.
  Structure is [documented below](#nested_or_conditions).

* `types` -
  (Optional)
  Restrict discovery to specific table type
  Structure is [documented below](#nested_types).

* `type_collection` -
  (Optional)
  Restrict discovery to categories of table types. Currently view, materialized view, snapshot and non-biglake external tables are supported.
  Possible values are: `BIG_QUERY_COLLECTION_ALL_TYPES`, `BIG_QUERY_COLLECTION_ONLY_SUPPORTED_TYPES`.


<a name="nested_or_conditions"></a>The `or_conditions` block supports:

* `min_age` -
  (Optional)
  Duration format. The minimum age a table must have before Cloud DLP can profile it. Value greater than 1.

* `min_row_count` -
  (Optional)
  Minimum number of rows that should be present before Cloud DLP profiles as a table.

<a name="nested_types"></a>The `types` block supports:

* `types` -
  (Optional)
  A set of BiqQuery table types
  Each value may be one of: `BIG_QUERY_TABLE_TYPE_TABLE`, `BIG_QUERY_TABLE_TYPE_EXTERNAL_BIG_LAKE`.

<a name="nested_cadence"></a>The `cadence` block supports:

* `schema_modified_cadence` -
  (Optional)
  Governs when to update data profiles when a schema is modified
  Structure is [documented below](#nested_schema_modified_cadence).

* `table_modified_cadence` -
  (Optional)
  Governs when to update profile when a table is modified.
  Structure is [documented below](#nested_table_modified_cadence).


<a name="nested_schema_modified_cadence"></a>The `schema_modified_cadence` block supports:

* `types` -
  (Optional)
  The type of events to consider when deciding if the table's schema has been modified and should have the profile updated. Defaults to NEW_COLUMN.
  Each value may be one of: `SCHEMA_NEW_COLUMNS`, `SCHEMA_REMOVED_COLUMNS`.

* `frequency` -
  (Optional)
  How frequently profiles may be updated when schemas are modified. Default to monthly
  Possible values are: `UPDATE_FREQUENCY_NEVER`, `UPDATE_FREQUENCY_DAILY`, `UPDATE_FREQUENCY_MONTHLY`.

<a name="nested_table_modified_cadence"></a>The `table_modified_cadence` block supports:

* `types` -
  (Optional)
  The type of events to consider when deciding if the table has been modified and should have the profile updated. Defaults to MODIFIED_TIMESTAMP
  Each value may be one of: `TABLE_MODIFIED_TIMESTAMP`.

* `frequency` -
  (Optional)
  How frequently data profiles can be updated when tables are modified. Defaults to never.
  Possible values are: `UPDATE_FREQUENCY_NEVER`, `UPDATE_FREQUENCY_DAILY`, `UPDATE_FREQUENCY_MONTHLY`.

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are exported:

* `id` - an identifier for the resource with format `{{parent}}/discoveryConfigs/{{name}}`

* `name` -
  Unique resource name for the DiscoveryConfig, assigned by the service when the DiscoveryConfig is created.

* `errors` -
  Output only. A stream of errors encountered when the config was activated. Repeated errors may result in the config automatically being paused. Output only field. Will return the last 100 errors. Whenever the config is modified this list will be cleared.
  Structure is [documented below](#nested_errors).

* `create_time` -
  Output only. The creation timestamp of a DiscoveryConfig.

* `update_time` -
  Output only. The last update timestamp of a DiscoveryConfig.

* `last_run_time` -
  Output only. The timestamp of the last time this config was executed


<a name="nested_errors"></a>The `errors` block contains:

* `details` -
  (Optional)
  Detailed error codes and messages.
  Structure is [documented below](#nested_details).

* `timestamp` -
  (Optional)
  The times the error occurred. List includes the oldest timestamp and the last 9 timestamps.


<a name="nested_details"></a>The `details` block supports:

* `code` -
  (Optional)
  The status code, which should be an enum value of google.rpc.Code.

* `message` -
  (Optional)
  A developer-facing error message, which should be in English. Any user-facing error message should be localized and sent in the google.rpc.Status.details field, or localized by the client.

* `details` -
  (Optional)
  A list of messages that carry the error details.

## Timeouts

This resource provides the following
[Timeouts](https://developer.hashicorp.com/terraform/plugin/sdkv2/resources/retries-and-customizable-timeouts) configuration options:

- `create` - Default is 20 minutes.
- `update` - Default is 20 minutes.
- `delete` - Default is 20 minutes.

## Import


DiscoveryConfig can be imported using any of these accepted formats:

* `{{parent}}/discoveryConfigs/{{name}}`
* `{{parent}}/{{name}}`


In Terraform v1.5.0 and later, use an [`import` block](https://developer.hashicorp.com/terraform/language/import) to import DiscoveryConfig using one of the formats above. For example:

```tf
import {
  id = "{{parent}}/discoveryConfigs/{{name}}"
  to = google_data_loss_prevention_discovery_config.default
}
```

When using the [`terraform import` command](https://developer.hashicorp.com/terraform/cli/commands/import), DiscoveryConfig can be imported using one of the formats above. For example:

```
$ terraform import google_data_loss_prevention_discovery_config.default {{parent}}/discoveryConfigs/{{name}}
$ terraform import google_data_loss_prevention_discovery_config.default {{parent}}/{{name}}
```
