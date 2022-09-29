---
description: REST and GraphQL API documentation on the Server collection in Directus.
readTime: 4 min read
pageClass: page-reference
---

# Server

> Provides detailed information about the project server, its schema, and its health.
> [Learn more about Projects](/getting-started/glossary#projects).

---

## Get OpenAPI Specification

Retrieve the OpenAPI spec for the current project.

::: tip Permissions

This OAS spec is based on the read permissions of the currently authenticated user.

:::

### Returns

Object conforming to [the OpenAPI Specification](https://swagger.io/specification)

### REST API

```
GET /server/specs/oas
```

### GraphQL

```
POST /graphql/system
```

```graphql
type Query {
	server_specs_oas: String
}
```

##### Example

```graphql
query {
	server_specs_oas
}
```

---

## Get GraphQL SDL

Retrieve the GraphQL SDL for the current project.

::: tip Permissions

The SDL is based on the permissions of the currently authenticated user.

:::

### Returns

GraphQL SDL file.

```graphql
type about_us {
  id: Int
  introduction: String
  our_process: String
  sales_email: String
  general_email: String
  primary_color: String
  secondary_color: String
  logo: directus_files
  mark: directus_files
}

type articles {
  id: Int
  status: String
	...
# etc
```

### REST API

```
GET /server/specs/graphql/
GET /server/specs/graphql/system
```

### GraphQL

```
POST /graphql/system
```

```graphql
type Query {
	server_specs_graphql(scope: graphql_sdl_scope): String
}
```

##### Example

```graphql
query {
	server_specs_graphql(scope: system)
}
```

---

## Ping

Ping... pong! 🏓

### Returns

Pong.

### REST API

```
GET /server/ping
```

### GraphQL

```
POST /graphql/system
```

```graphql
type Query {
	server_ping: String
}
```

##### Example

```graphql
query {
	server_ping
}
```

---

## Info

Information about the current installation.

::: tip Permissions

The public information is returned for everybody. Admin users get additional information (see below).

:::

### Returns

`project` **object**\
Public information about the project. Used to render the Admin App public pages.

See [the settings object](/reference/system/settings#the-settings-object) for more information on the individual
properties of the `project` object.

Logged in users also get the following information:

`rateLimit` **false | object**\
Whether or not the rate limiter is enabled.

`rateLimit.points` **number**\
If rate-limiter is enabled, amount of allowed points per duration

`rateLimit.duration` **number**\
If rate-limiter is enabled, duration in seconds in which points are counted

Admin users also get the following information:

`directus.version` **string**\
Current version of Directus used.

`node.version` **string**\
Current version of Node used.

`node.uptime` **integer**\
How long the current process has been running.

`os.type` **string**\
What type of operation system is used.

`os.version` **string**\
What version of the operation system is used.

`os.uptime` **string**\
How long the operating system has been up.

`os.totalmem` **string**\
How much memory is available on the operating system.

### REST API

```
GET /server/info
```

### GraphQL

```
POST /graphql/system
```

```graphql
type Query {
	server_info: server_info
}
```

##### Example

```graphql
query {
	server_info {
		directus {
			version
		}
	}
}
```

---

## Health

Get the current health status of the server.

The `/server/health` endpoint shows you a general health status for the server and all connected (third party) services,
such as Redis or S3.

The output is based on the "Health Check Response for HTTP APIs" draft spec:
[Health Check Response Format for HTTP APIs Draft Specification](https://tools.ietf.org/id/draft-inadarei-api-health-check-05.html).

This endpoint can be used to ensure a healthy system when running in a horizontally scaled setup, like Kubernetes,
Google Cloud Platform or AWS Elastic Beanstalk.

By default, the endpoint only returns a `status` of `ok`, `warn` or `error`. By authenticating as an admin, it will
return more in-depth information about the current health status of the system.

```json
// Response

// Non-admin
{
  "status": "ok"
}

// Admin
{
  "status": "ok",
  "releaseId": "9.0.0",
  "serviceId": "3292c816-ae02-43b4-ba91-f0bb549f040c",
  "checks": {
    "pg:responseTime": [
      {
        "status": "ok",
        "componentType": "datastore",
        "observedUnit": "ms",
        "observedValue": 0.489
      }
    ],
    "pg:connectionsAvailable": [
      {
        "status": "ok",
        "componentType": "datastore",
        "observedValue": 2
      }
    ],
    "pg:connectionsUsed": [
      {
        "status": "ok",
        "componentType": "datastore",
        "observedValue": 0
      }
    ],
    "storage:local:responseTime": [
      {
        "status": "ok",
        "componentType": "objectstore",
        "observedValue": 1.038,
        "observedUnit": "ms"
      }
    ],
    "email:connection": [
      {
        "status": "ok",
        "componentType": "email"
      }
    ]
  }
}
```

### Returns

`status` **string**\
One of `ok`, `warn`, `error`.

Authenticated admin users also get the following information:

`releaseId` **string**\
Directus version in use.

`serviceId` **string**\
UUID of the current Directus instance.

`checks` **array**\
Array with the status of all individually connected services.

### REST API

```
GET /server/health
```

### GraphQL

```
POST /graphql/system
```

```graphql
type Query {
	server_health: JSON
}
```

##### Example

```graphql
query {
	server_health
}
```

---

## Retrieve Schema Snapshot

Retrieve the current schema. This endpoint is only available to admin users.

### Query Parameters

Supports the [export](/reference/query#export) query parameter.

### Returns

Returns the JSON object containing schema details by default, or downloads it in an alternative format when `export`
query parameter is used.

### REST API

```
GET /server/schema/snapshot
```

##### Example

```
// returns JSON object
GET /server/schema/snapshot

// download as YAML file
GET /server/schema/snapshot?export=yaml
```

### GraphQL

n/a

---

## Apply Schema Difference

Apply schema difference based on JSON request body. This endpoint is only available to admin users.

### Query Parameters

This endpoint doesn't currently support any query parameters.

### Request Body

JSON object containing hash and diffs of [collections](/reference/system/collections#the-collection-object),
[fields](/reference/system/fields#the-field-object), and [relations](/reference/system/relations#the-relation-object) to
apply.

::: tip Hashes

The hash is based on the target instance's schema and version. It is used to safeguard against changes that may happen
after the current diff was generated which can potentially incur unexpected side effects when applying the diffs without
this safeguard. Each diffs will also contain a hash, except for diffs of kind "N" (create).

:::

### Returns

Empty body.

### REST API

```
POST /server/schema/apply
```

##### Example

```
POST /server/schema/apply

{
    "hash": "327374bd",
    "diff": {
      "collections": [
        {
          "collection": "test",
          "diff": [
            {
              "kind": "N",
              "rhs": {
                "collection": "test",
                "meta": {
                  "accountability": "all",
                  "archive_app_filter": true,
                  "archive_field": null,
                  "archive_value": null,
                  "collapse": "open",
                  "collection": "test",
                  "color": null,
                  "display_template": null,
                  "group": null,
                  "hidden": false,
                  "icon": null,
                  "item_duplication_fields": null,
                  "note": null,
                  "singleton": false,
                  "sort": null,
                  "sort_field": null,
                  "translations": null,
                  "unarchive_value": null
                },
                "schema": {
                  "name": "test"
                }
              }
            }
          ]
        }
      ],
      "fields": [
        {
          "collection": "test",
          "field": "id",
          "diff": [
            {
              "kind": "N",
              "rhs": {
                "collection": "test",
                "field": "id",
                "meta": {
                  "collection": "test",
                  "conditions": null,
                  "display": null,
                  "display_options": null,
                  "field": "id",
                  "group": null,
                  "hidden": true,
                  "interface": "input",
                  "note": null,
                  "options": null,
                  "readonly": true,
                  "required": false,
                  "sort": null,
                  "special": null,
                  "translations": null,
                  "validation": null,
                  "validation_message": null,
                  "width": "full"
                },
                "schema": {
                  "data_type": "integer",
                  "default_value": "nextval('test_id_seq'::regclass)",
                  "foreign_key_column": null,
                  "foreign_key_table": null,
                  "generation_expression": null,
                  "has_auto_increment": true,
                  "is_generated": false,
                  "is_nullable": false,
                  "is_primary_key": true,
                  "is_unique": true,
                  "max_length": null,
                  "name": "id",
                  "numeric_precision": 32,
                  "numeric_scale": 0,
                  "table": "test"
                },
                "type": "integer"
              }
            }
          ]
        },
        {
          "collection": "test",
          "field": "title",
          "diff": [
            {
              "kind": "N",
              "rhs": {
                "collection": "test",
                "field": "title",
                "meta": {
                  "collection": "test",
                  "conditions": null,
                  "display": null,
                  "display_options": null,
                  "field": "title",
                  "group": null,
                  "hidden": false,
                  "interface": "input",
                  "note": null,
                  "options": null,
                  "readonly": false,
                  "required": false,
                  "sort": null,
                  "special": null,
                  "translations": null,
                  "validation": null,
                  "validation_message": null,
                  "width": "full"
                },
                "schema": {
                  "data_type": "character varying",
                  "default_value": null,
                  "foreign_key_column": null,
                  "foreign_key_table": null,
                  "generation_expression": null,
                  "has_auto_increment": false,
                  "is_generated": false,
                  "is_nullable": true,
                  "is_primary_key": false,
                  "is_unique": false,
                  "max_length": 255,
                  "name": "title",
                  "numeric_precision": null,
                  "numeric_scale": null,
                  "table": "test"
                },
                "type": "string"
              }
            }
          ]
        }
      ],
      "relations": []
    }
  }
```

### GraphQL

n/a

---

## Retrieve Schema Difference

Compares and retrieve the difference of the current schema vs the schema in JSON request body. This endpoint is only
available to admin users.

Alternatively, upload a JSON or YAML schema file. Relies on a `multipart/form-data` encoded request like regular file
uploads. Check [Upload a File](/reference/files#upload-a-file) for more information.

### Query Parameters

This endpoint doesn't currently support any query parameters.

### Request Body

JSON object containing [collections](/reference/system/collections#the-collection-object),
[fields](/reference/system/fields#the-field-object), and [relations](/reference/system/relations#the-relation-object) to
apply.

Alternatively, send a JSON or YAML schema file in a `multipart/form-data` request. See
[Upload a File](/reference/files#upload-a-file) for more information.

### Returns

Empty body.

### REST API

```
POST /server/schema/diff
```

##### Example

```
POST /server/schema/diff

Content-Type: multipart/form-data; charset=utf-8; boundary=__X_BOUNDARY__
Content-Length: 3442422

--__X_BOUNDARY__
Content-Disposition: form-data; name="file"; filename="schema.yaml"
Content-Type: text/yaml

version: 1
directus: 9.14.0
collections:
  - collection: articles
    meta:
      accountability: all
      archive_app_filter: true
      archive_field: null
      archive_value: null
      collapse: open
      collection: articles
      color: null
      display_template: null
      group: null
      hidden: false
      icon: null
      item_duplication_fields: null
      note: null
      singleton: false
      sort: null
      sort_field: null
      translations: null
      unarchive_value: null
    schema:
      comment: null
      name: articles
      schema: public
fields:
  - collection: articles
    field: id
    meta:
      collection: articles
      conditions: null
      display: null
      display_options: null
      field: id
      group: null
      hidden: true
      interface: input
      note: null
      options: null
      readonly: true
      required: false
      sort: null
      special: null
      translations: null
      validation: null
      validation_message: null
      width: full
    schema:
      comment: null
      data_type: integer
      default_value: nextval('articles_id_seq'::regclass)
      foreign_key_column: null
      foreign_key_schema: null
      foreign_key_table: null
      generation_expression: null
      has_auto_increment: true
      is_generated: false
      is_nullable: false
      is_primary_key: true
      is_unique: true
      max_length: null
      name: id
      numeric_precision: 32
      numeric_scale: 0
      schema: public
      table: articles
    type: integer
  - collection: articles
    field: title
    meta:
      collection: articles
      conditions: null
      display: null
      display_options: null
      field: title
      group: null
      hidden: false
      interface: input
      note: null
      options: null
      readonly: false
      required: false
      sort: null
      special: null
      translations: null
      validation: null
      validation_message: null
      width: full
    schema:
      comment: null
      data_type: character varying
      default_value: null
      foreign_key_column: null
      foreign_key_schema: null
      foreign_key_table: null
      generation_expression: null
      has_auto_increment: false
      is_generated: false
      is_nullable: true
      is_primary_key: false
      is_unique: false
      max_length: 255
      name: title
      numeric_precision: null
      numeric_scale: null
      schema: public
      table: articles
    type: string
relations: []

...
```

### GraphQL

n/a
