---
layout: "mongodbatlas"
page_title: "MongoDB Atlas: organizations"
sidebar_current: "docs-mongodbatlas-organizations"
description: |-
    Describes Organizations.
---

# Data Source: mongodbatlas_organizations

`mongodbatlas_organizations` describe all MongoDB Atlas Organizations. This represents organizations that have been created.


## Example Usage

```terraform
data "mongodbatlas_organizations" "test" {
  page_num = 1
  items_per_page = 5
}
```

## Argument Reference
* `page_num` - (Optional)  	The page to return. Defaults to `1`.
* `items_per_page` - (Optional) Number of items to return per page, up to a maximum of 500. Defaults to `100`.


* `id` - Autogenerated Unique ID for this data source.
* `total_count` - Represents the total number of organizations

* `results` - A list where each item represents an Organization.


### Organization

* `name` - Human-readable label that identifies the organization.
* `id` - Unique 24-hexadecimal digit string that identifies the organization.
* `is_deleted` - Flag that indicates whether this organization has been deleted.
  
See [MongoDB Atlas API - Organizations](https://www.mongodb.com/docs/atlas/reference/api-resources-spec/#tag/Organizations/operation/listOrganizations)  Documentation for more information.
