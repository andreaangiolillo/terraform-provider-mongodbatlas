---
layout: "mongodbatlas"
page_title: "MongoDB Atlas: cluster"
sidebar_current: "docs-mongodbatlas-datasource-clusters"
description: |-
    Describe all Advanced Clusters in Project.
---

# Data Source: mongodbatlas_clusters

`mongodbatlas_cluster` describes all Advanced Clusters by the provided project_id. The data source requires your Project ID.

-> **NOTE:** Groups and projects are synonymous terms. You may find group_id in the official documentation.

~> **IMPORTANT:**
<br> &#8226; Changes to cluster configurations can affect costs. Before making changes, please see [Billing](https://docs.atlas.mongodb.com/billing/).
<br> &#8226; If your Atlas project contains a custom role that uses actions introduced in a specific MongoDB version, you cannot create a cluster with a MongoDB version less than that version unless you delete the custom role.

## Example Usage

```terraform
resource "mongodbatlas_cluster" "example" {
  project_id   = "<YOUR-PROJECT-ID>"
  name         = "cluster-test"
  cluster_type = "REPLICASET"

  replication_specs {
    region_configs {
      electable_specs {
        instance_size = "M5"
      }
      provider_name         = "TENANT"
      backing_provider_name = "AWS"
      region_name           = "US_EAST_1"
      priority              = 7
    }
  }
}

data "mongodbatlas_clusters" "example" {
  project_id = mongodbatlas_cluster.example.project_id
}
```

## Argument Reference

* `project_id` - (Required) The unique ID for the project to get the clusters.

## Attributes Reference

In addition to all arguments above, the following attributes are exported:

* `id` - The cluster ID.
* `results` - A list where each represents a Cluster. See below for more details.

### Advanced Cluster

* `bi_connector_config` - Configuration settings applied to BI Connector for Atlas on this cluster. See [below](#bi_connector_config). **NOTE** Prior version of provider had parameter as `bi_connector`
* `cluster_type` - Type of the cluster that you want to create.
* `disk_size_gb` - Capacity, in gigabytes, of the host's root volume.
* `encryption_at_rest_provider` - Possible values are AWS, GCP, AZURE or NONE.
* `labels` - Configuration for the collection of key-value pairs that tag and categorize the cluster. See [below](#labels).
* `mongo_db_major_version` - Version of the cluster to deploy.
* `pit_enabled` - Flag that indicates if the cluster uses Continuous Cloud Backup.
* `replication_specs` - Configuration for cluster regions and the hardware provisioned in them. See [below](#replication_specs)
* `root_cert_type` - Certificate Authority that MongoDB Atlas clusters use.
* `termination_protection_enabled` - Flag that indicates whether termination protection is enabled on the cluster. If set to true, MongoDB Cloud won't delete the cluster. If set to false, MongoDB Cloud will delete the cluster.
* `version_release_system` - Release cadence that Atlas uses for this cluster.
* `advanced_configuration` - Get the advanced configuration options. See [Advanced Configuration](#advanced-configuration) below for more details.


### bi_connector_config

Specifies BI Connector for Atlas configuration.

* `enabled` - Specifies whether or not BI Connector for Atlas is enabled on the cluster.l
* `read_preference` - Specifies the read preference to be used by BI Connector for Atlas on the cluster. Each BI Connector for Atlas read preference contains a distinct combination of [readPreference](https://docs.mongodb.com/manual/core/read-preference/) and [readPreferenceTags](https://docs.mongodb.com/manual/core/read-preference/#tag-sets) options. For details on BI Connector for Atlas read preferences, refer to the [BI Connector Read Preferences Table](https://docs.atlas.mongodb.com/tutorial/create-global-writes-cluster/#bic-read-preferences).

### labels

Key-value pairs that tag and categorize the cluster. Each key and value has a maximum length of 255 characters.  You cannot set the key `Infrastructure Tool`, it is used for internal purposes to track aggregate usage.

* `key` - The key that you want to write.
* `value` - The value that you want to write.


### replication_specs

* `num_shards` - Provide this value if you set a `cluster_type` of SHARDED or GEOSHARDED.
* `region_configs` - Configuration for the hardware specifications for nodes set for a given regionEach `region_configs` object describes the region's priority in elections and the number and type of MongoDB nodes that Atlas deploys to the region. Each `region_configs` object must have either an `analytics_specs` object, `electable_specs` object, or `read_only_specs` object. See [below](#region_configs)
*  `container_id` - A key-value map of the Network Peering Container ID(s) for the configuration specified in `region_configs`. The Container ID is the id of the container either created programmatically by the user before any clusters existed in a project or when the first cluster in the region (AWS/Azure) or project (GCP) was created.  The syntax is `"providerName:regionName" = "containerId"`. Example `AWS:US_EAST_1" = "61e0797dde08fb498ca11a71`.
* `zone_name` - Name for the zone in a Global Cluster.


### region_configs

* `analytics_specs` - Hardware specifications for [analytics nodes](https://docs.atlas.mongodb.com/reference/faq/deployment/#std-label-analytics-nodes-overview) needed in the region. See [below](#specs)
* `auto_scaling` - Configuration for the Collection of settings that configures auto-scaling information for the cluster. See [below](#auto_scaling)
* `analytics_auto_scaling` - Configuration for the Collection of settings that configures analytis-auto-scaling information for the cluster. See [below](#analytics_auto_scaling)
* `backing_provider_name` - Cloud service provider on which you provision the host for a multi-tenant cluster.
* `electable_specs` - Hardware specifications for electable nodes in the region.
* `priority` -  Election priority of the region.
* `provider_name` - Cloud service provider on which the servers are provisioned.
* `read_only_specs` - Hardware specifications for read-only nodes in the region. See [below](#specs)
* `region_name` - Physical location of your MongoDB cluster.

### specs

* `disk_iops` - Target throughput (IOPS) desired for AWS storage attached to your cluster.
* `ebs_volume_type` - Type of storage you want to attach to your AWS-provisioned cluster.
  * `STANDARD` volume types can't exceed the default IOPS rate for the selected volume size.
  * `PROVISIONED` volume types must fall within the allowable IOPS range for the selected volume size.
* `instance_size` - Hardware specification for the instance sizes in this region.
* `node_count` - Number of nodes of the given type for MongoDB Atlas to deploy to the region.

### auto_scaling

* `disk_gb_enabled` - Flag that indicates whether this cluster enables disk auto-scaling.
* `compute_enabled` - Flag that indicates whether instance size auto-scaling is enabled.
* `compute_scale_down_enabled` - Flag that indicates whether the instance size may scale down.
* `compute_min_instance_size` - Minimum instance size to which your cluster can automatically scale (such as M10).
* `compute_max_instance_size` - Maximum instance size to which your cluster can automatically scale (such as M40).

### analytics_auto_scaling

* `disk_gb_enabled` - Flag that indicates whether this cluster enables disk auto-scaling.
* `compute_enabled` - Flag that indicates whether instance size auto-scaling is enabled.
* `compute_scale_down_enabled` - Flag that indicates whether the instance size may scale down.
* `compute_min_instance_size` - Minimum instance size to which your cluster can automatically scale (such as M10).
* `compute_max_instance_size` - Maximum instance size to which your cluster can automatically scale (such as M40).

#### Advanced Configuration

* `default_read_concern` - [Default level of acknowledgment requested from MongoDB for read operations](https://docs.mongodb.com/manual/reference/read-concern/) set for this cluster. MongoDB 4.4 clusters default to [available](https://docs.mongodb.com/manual/reference/read-concern-available/).
* `default_write_concern` -  [Default level of acknowledgment requested from MongoDB for write operations](https://docs.mongodb.com/manual/reference/write-concern/) set for this cluster. MongoDB 4.4 clusters default to [1](https://docs.mongodb.com/manual/reference/write-concern/).
* `fail_index_key_too_long` - When true, documents can only be updated or inserted if, for all indexed fields on the target collection, the corresponding index entries do not exceed 1024 bytes. When false, mongod writes documents that exceed the limit but does not index them.
* `javascript_enabled` - When true, the cluster allows execution of operations that perform server-side executions of JavaScript. When false, the cluster disables execution of those operations.
* `minimum_enabled_tls_protocol` - Sets the minimum Transport Layer Security (TLS) version the cluster accepts for incoming connections.Valid values are:

  - TLS1_0
  - TLS1_1
  - TLS1_2

* `no_table_scan` - When true, the cluster disables the execution of any query that requires a collection scan to return results. When false, the cluster allows the execution of those operations.
* `oplog_size_mb` - The custom oplog size of the cluster. Without a value that indicates that the cluster uses the default oplog size calculated by Atlas.
* `oplog_min_retention_hours` - Minimum retention window for cluster's oplog expressed in hours. A value of null indicates that the cluster uses the default minimum oplog window that MongoDB Cloud calculates.
* `sample_size_bi_connector` - Number of documents per database to sample when gathering schema information. Defaults to 100. Available only for Atlas deployments in which BI Connector for Atlas is enabled.
* `sample_refresh_interval_bi_connector` - Interval in seconds at which the mongosqld process re-samples data to create its relational schema. The default value is 300. The specified value must be a positive integer. Available only for Atlas deployments in which BI Connector for Atlas is enabled.


## Attributes Reference

In addition to all arguments above, the following attributes are exported:

* `cluster_id` - The cluster ID.
*  `mongo_db_version` - Version of MongoDB the cluster runs, in `major-version`.`minor-version` format.
* `id` -	The Terraform's unique identifier used internally for state management.
* `connection_strings` - Set of connection strings that your applications use to connect to this cluster. More info in [Connection-strings](https://docs.mongodb.com/manual/reference/connection-string/). Use the parameters in this object to connect your applications to this cluster. To learn more about the formats of connection strings, see [Connection String Options](https://docs.atlas.mongodb.com/reference/faq/connection-changes/). NOTE: Atlas returns the contents of this object after the cluster is operational, not while it builds the cluster.

  **NOTE** Connection strings must be returned as a list, therefore to refer to a specific attribute value add index notation. Example: mongodbatlas_advanced_cluster.cluster-test.connection_strings.0.standard_srv

  Private connection strings may not be available immediately as the reciprocal connections may not have finalized by end of the Terraform run. If the expected connection string(s) do not contain a value a terraform refresh may need to be performed to obtain the value. One can also view the status of the peered connection in the [Atlas UI](https://docs.atlas.mongodb.com/security-vpc-peering/).

  - `connection_strings.standard` -   Public mongodb:// connection string for this cluster.
  - `connection_strings.standard_srv` - Public mongodb+srv:// connection string for this cluster. The mongodb+srv protocol tells the driver to look up the seed list of hosts in DNS. Atlas synchronizes this list with the nodes in a cluster. If the connection string uses this URI format, you don’t need to append the seed list or change the URI if the nodes change. Use this URI format if your driver supports it. If it doesn’t  , use connectionStrings.standard.
  - `connection_strings.aws_private_link` -  [Private-endpoint-aware](https://docs.atlas.mongodb.com/security-private-endpoint/#private-endpoint-connection-strings) mongodb://connection strings for each interface VPC endpoint you configured to connect to this cluster. Returned only if you created a AWS PrivateLink connection to this cluster. **DEPRECATED** Use `connection_strings.private_endpoint[n].connection_string` instead.
  - `connection_strings.aws_private_link_srv` - [Private-endpoint-aware](https://docs.atlas.mongodb.com/security-private-endpoint/#private-endpoint-connection-strings) mongodb+srv://connection strings for each interface VPC endpoint you configured to connect to this cluster. Returned only if you created a AWS PrivateLink connection to this cluster. Use this URI format if your driver supports it. If it doesn’t, use connectionStrings.awsPrivateLink. **DEPRECATED** Use `connection_strings.private_endpoint[n].srv_connection_string` instead.
  - `connection_strings.private` -   [Network-peering-endpoint-aware](https://docs.atlas.mongodb.com/security-vpc-peering/#vpc-peering) mongodb://connection strings for each interface VPC endpoint you configured to connect to this cluster. Returned only if you created a network peering connection to this cluster.
  - `connection_strings.private_srv` -  [Network-peering-endpoint-aware](https://docs.atlas.mongodb.com/security-vpc-peering/#vpc-peering) mongodb+srv://connection strings for each interface VPC endpoint you configured to connect to this cluster. Returned only if you created a network peering connection to this cluster.
  - `connection_strings.private_endpoint` - Private endpoint connection strings. Each object describes the connection strings you can use to connect to this cluster through a private endpoint. Atlas returns this parameter only if you deployed a private endpoint to all regions to which you deployed this cluster's nodes.
  - `connection_strings.private_endpoint.#.connection_string` - Private-endpoint-aware `mongodb://`connection string for this private endpoint.
  - `connection_strings.private_endpoint.#.srv_connection_string` - Private-endpoint-aware `mongodb+srv://` connection string for this private endpoint. The `mongodb+srv` protocol tells the driver to look up the seed list of hosts in DNS . Atlas synchronizes this list with the nodes in a cluster. If the connection string uses this URI format, you don't need to: Append the seed list or Change the URI if the nodes change. Use this URI format if your driver supports it. If it doesn't, use `connection_strings.private_endpoint[n].connection_string`
  - `connection_strings.private_endpoint.#.srv_shard_optimized_connection_string` - Private endpoint-aware connection string optimized for sharded clusters that uses the `mongodb+srv://` protocol to connect to MongoDB Cloud through a private endpoint. If the connection string uses this Uniform Resource Identifier (URI) format, you don't need to change the Uniform Resource Identifier (URI) if the nodes change. Use this Uniform Resource Identifier (URI) format if your application and Atlas cluster supports it. If it doesn't, use and consult the documentation for connectionStrings.privateEndpoint[n].srvConnectionString.
  - `connection_strings.private_endpoint.#.type` - Type of MongoDB process that you connect to with the connection strings. Atlas returns `MONGOD` for replica sets, or `MONGOS` for sharded clusters.
  - `connection_strings.private_endpoint.#.endpoints` - Private endpoint through which you connect to Atlas when you use `connection_strings.private_endpoint[n].connection_string` or `connection_strings.private_endpoint[n].srv_connection_string`
  - `connection_strings.private_endpoint.#.endpoints.#.endpoint_id` - Unique identifier of the private endpoint.
  - `connection_strings.private_endpoint.#.endpoints.#.provider_name` - Cloud provider to which you deployed the private endpoint. Atlas returns `AWS` or `AZURE`.
  - `connection_strings.private_endpoint.#.endpoints.#.region` - Region to which you deployed the private endpoint.
* `paused` - Flag that indicates whether the cluster is paused or not.
* `state_name` - Current state of the cluster. The possible states are:

See detailed information for arguments and attributes: [MongoDB API Advanced Clusters](https://docs.atlas.mongodb.com/reference/api/cluster-advanced/get-all-cluster-advanced/)
