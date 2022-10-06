---
description:
  Operations are the individual actions in a flow. They enable you to do things like manage data within Directus,
  transform the flow's data, send information off to outside services, set conditional logic, trigger other flows, and
  beyond!
readTime: 5 min read
---

# Operations

> Operations are the individual actions in a flow. They enable you to do things like manage data within Directus,
> transform the flow's data, send information off to outside services, set conditional logic, trigger other flows, _and
> beyond!_

:::tip Before You Begin

On this page, we'll explain what each operation does, the kind of value it appends to the Flow Object, how to make use
of its configuration options, as well as any well as other relevant tips. We will assume you have read the documentation
on [Flows](/configuration/flows) and [Triggers](/configuration/flows/triggers).

:::

## Condition

![Condition](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/condition-20220603A.webp)

A condition operation lets you use a [filter](/reference/filter-rules) to determine the next operation chain in the
flow, based on the value of data within the Flow Object.

- **Condition Rules** — Create conditions with [Filter Rules](/reference/filter-rules).

If the Filter Rule is configured properly, a `null` value will be appended under its Flow Object operation key,
regardless of if the condition was met or not. If the condition is misconfigured, it will append an array containing an
object you can use to help debug the misconfiguration. This object has two keys of importance `_original`, which will
have the Flow Object, and `details`, which will provide information to help identify the misconfiguration.

:::warning

When using a [Event Hook](/configuration/flows/triggers.md#event-hook) configured to be **Action (Blocking)**, if your
flow ends with a condition that executes with a `reject` path, it will cancel your database transaction.

:::

## Run Script

<video autoplay playsinline muted loop controls title="Run Script">
	<source src="" type="video/mp4" />
</video>

Run Script lets you add a custom script to your flow using vanilla JavaScript or TypeScript.

The operation provides a default function template. Its _optional_ `data` parameter takes in the existing Flow Object as
an argument. The function's returned value is appended under the Run Script operation key.

For example, let's say you have this function in a script operation, named `myScript`.

```JSON
// A preceding operation key from the Flow Object
{
  "previousOperation": {
    "value": 5
  }
}
```

```TypeScript
// Function in the myScript operation
module.exports = function(data) {
  return {
    timesTwo: data.previousOperation.value * 2
  }
}

```

The returned value will be appended under the `myScript` operation key.

```JSON
{
  "previousOperation": {
    "value": 5
  },
  "myScript": {
    "timesTwo": 10
  }
}

```

:::tip Throwing Errors

If you throw an error in a Run Script operation, it will immediately break your flow chain and stop execution of
subsequent flows. If you used a ["Blocking" Event hook](/configuration/flows/triggers.md#event-hook), throwing an error
will cancel the event transaction.

:::

:::tip

We just showed you how to pass the Flow Object into a Run Script operation, run a simple calculation on its data, and
append the new data back onto the Flow Object. But remember, it's your function and you can use it as se fit! You're not
required to input `data` or return a value.

:::

## Create Data

![Create Data](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/create-data-20220603A.webp)

This operation creates item(s) in a collection.

- **Collection** — Select the collection you'd like to create items in.
- **Permissions** — Select the scope of permissions used for this operation.
- **Emit Events** — Toggle whether the event is emitted.
- **Payload** — Define the payload to create item(s) in a collection.

:::tip

**Emit Events** toggles the operation's "visibility" throughout Directus. For example, if togged on, this operation will
trigger relevant event hooks in other flows or custom extensions. If toggled off, the operation will not trigger other
event hooks. This is useful in the situation where you have a flow being triggered by `<collection>.items.create` which
contains an operation that tries to create another item in that `<collection>`. Typically, this would throw an infinite
loop of created items. However, if you toggle **Emit Events** off, then the operation within the flow no longer triggers
other event hooks.

:::

:::tip

Make sure the operation is scoped with the [permissions](/configuration/users-roles-permissions) necessary to create
items.

:::

:::tip

To learn about payload requirements when creating an item, see [API Reference > Items](/reference/items).

:::

## Delete Data

![Delete Data](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/delete-data-20220603A.webp)

This operation deletes item(s) from a collection by either ID or query.

- **Collection** — Use the dropdown menu to select the Collection you'd like to delete Items from.
- **Permissions** — Set the scope of permissions used for this Operation.
- **Emit Events** — Toggle whether the event is emitted.
- **IDs** — Set Item IDs and press enter to confirm. Click the ID to remove.
- **Query** — Select Items to delete with a query. To learn more, see [Filter Rules](/reference/filter-rules).

:::tip

**Emit Events** toggles the operation's "visibility" throughout Directus. For example, if togged on, this operation will
trigger relevant event hooks in other flows or custom extensions. If toggled off, the operation will not trigger other
event hooks. This is useful in the situation where you have a flow being triggered by `<collection>.items.delete` which
contains an operation that then tries to delete another item in that `<collection>`. Typically, this would throw an
infinite loop of deleted items. However, if you toggle **Emit Events** off, then the operation within the flow no longer
triggers other event hooks.

:::

:::tip

**Emit Events** toggles the operation's "visibility" throughout Directus. For example, if togged on, this operation will
trigger relevant event hooks in other flows or custom extensions. If toggled off, the operation will not trigger event
hooks or any another behavior.

:::

:::tip

Make sure the operation is scoped with the [permissions](/configuration/users-roles-permissions) necessary to delete
items.

:::

## Read Data

![Read Data](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/read-data-20220603A.webp)

This operation reads item(s) from a collection and adds them onto the Flow Object. You may select Items by their ID or
by running a query.

- **Permissions** — Set the scope of permissions used for this Operation.
- **Collections** — Select the Collection from which you'd like to read Items.
- **IDs** — Input the ID for Items you wish to read and press enter. Click the ID to remove.
- **Query** — Select the Items with a query. To learn more, see [Filter Rules](/reference/filter-rules).
- **Emit Events** — Toggle whether the event is emitted.

:::tip

**Emit Events** toggles the operation's "visibility" throughout Directus. For example, if togged on, this operation will
trigger relevant event hooks in other flows or custom extensions. If toggled off, the operation will not trigger other
event hooks. This is useful in the situation where you have a flow being triggered by `<collection>.items.read` which
contains an operation that then tries to read another item in that `<collection>`. Typically, this would throw an
infinite loop of read items. However, if you toggle **Emit Events** off, then the operation within the flow no longer
triggers other event hooks.

:::

:::tip

**Emit Events** toggles the operation's "visibility" throughout Directus. For example, if togged on, this operation will
trigger relevant event hooks in other flows or custom extensions. If toggled off, the operation will not trigger event
hooks or any another behavior.

:::

## Update Data

![Update Data](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/update-data-20220603A.webp)

This operation updates item(s) in a collection. You may select item(s) to update by their ID or by running a query.

- **Collections** — Select the Collection from which you'd like to read Items.
- **Permissions** — Set the Role that this Operation will inherit permissions from.
- **Emit Events** — Toggle whether the event is emitted.
- **IDs** — Input the ID for Item(s) you wish to read and press enter. Click the ID to remove.
- **Payload** — Update Items in a Collection. To learn more, see [API > Items](/reference/items).
- **Query** — Select Items to update with a query. To learn more, see [Filter Rules](/reference/filter-rules).

:::tip

**Emit Events** toggles the operation's "visibility" throughout Directus. For example, if togged on, this operation will
trigger relevant event hooks in other flows or custom extensions. If toggled off, the operation will not trigger other
event hooks. This is useful in the situation where you have a flow being triggered by `<collection>.items.update` which
contains an operation that then tries to update another item in that `<collection>`. Typically, this would throw an
infinite loop of updated items. However, if you toggle **Emit Events** off, then the operation within the flow no longer
triggers other event hooks.

:::

## Log to Console

![Log to Console](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/log-to-console-20220603A.webp)

This Operation outputs something to the server-side console as well as the [Log Panel](/configuration/flows#logs) within
the Data Studio. This is a key tool for troubleshooting Flow configuration. A Log operation's key will have a null value
on the Flow Object.

- **Message** — Sets a [log message](/configuration/flows#logs).

## Send Email

![Send Email](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/send-email-20220603A.webp)

This operation sends off email(s). Flow Object keys can be used as variables, which means you can use an array of emails
from a previous step in Flows, or dynamically read stored data into the subject or body of the email.

If the operation executes successfully, a `null` value will be appended onto its operation key.

- **To** — Sets the email addresses. Hit `↵` to save the email. Click an email to remove it.
- **Subject** — Set the subject line.
- **Body** — Use a Markdown or WYSIWYG editor to create the email body.

:::tip Batch Emails

You can input an array of emails in the `To` input option to send off multiple emails.

:::

:::tip

If you are testing out this Operation locally from `localhost:8080`, be sure to check your spam box, because your email
provider may send it there automatically.

:::

## Send Notification

![Send Notification](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/send-notification-20220603A.webp)

This operation sends a notification to an app user. Flow Object keys can be used as variables, which means you can use
an array of user IDs from a previous step in Flows, or dynamically read stored data into the notifications.

If the operation executes successfully, a list containing the IDs of all sent notifications generated is appended under
this operation's key.

- **Users** — Define a User by their primary key UUID. Use [Flow keys](/configuration/flows#the-flow-object) to set this
  dynamically.
- **Permissions** — Define the Role that this Operation will inherit permissions from.
- **Title** — Set the notification title.
- **Message** — Set the main body of the notification.

:::tip Batch Notifications

You can input an array of UUIDs in the `To` input option to send off multiple emails.

:::

## Webhook / Request URL

![Webhook / Request URL](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/webhook-20220603A.webp)

This operation makes a request to another URL. You can make any HTTP request needed. WHen the operation completes
successfully, the response is appended under the operation's key.

- **Method** — Choose to make a GET, POST, PATCH, DELETE, or other type of request.
- **URL** — Define the URL to send the request to.
- **Headers** — Create a new `header:value` to pass along with the request.
- **Request Body** — Set the request body data.

## Sleep

![Sleep](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/sleep-20220603A.webp)

This operation creates a delay in the Flow for a given amount of milliseconds, then continues to the next operation.
Once finished, a `null` value is appended under its operation key.

- **Milliseconds** — Define the number of milliseconds the Operation will pause.

## Transform Payload

![Transform Payload](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/transform-payload-20220603A.webp)

This operation lets you custom define your own JSON payload and append it to the Flow Object for use in subsequent
operations. You can hard-code the values or use data from previously generated keys on the
[Flow Object](/configuration/flows#the-flow-object). This enables you to take multiple sources of data and "tidy them
up" into a single object.

- **JSON** — Define JSON to insert into the Flow Object.

For example, let's say your final payload needs some data from a `users_collection`, `widgets_collection` and some 3rd
party resource which processes the data. You can add a Read Items operation for `collection_a`, another Read Items
operation for `collection_b`, and a Webhook operation for the third party resource, then create a Transform JSON
operation like so:

```
{
	"note": "You can add a hard-coded value!",
	"name": {{users_collection.username}},
	"widget_id": {{widgets_collection.id}},
	"results": {{webhookKey.subnestedValue}}
}
```

Your defined JSON payload is appended under the operation's key on the Flow Object for use in subsequent operations.

## Trigger Flow

![Trigger Flow](https://cdn.directus.io/docs/v9/configuration/flows/operations/operations-20220603A/trigger-flow-20220603A.webp)

This Operation starts another Flow and _(optionally)_ passes data into it. It should be used in combination with the
[Another Flow](/configuration/flows/triggers#another-flow) Trigger.

- **Flow** — Define a flow by its primary key UUID.
- **Payload** — Define JSON to pass in the request to the triggered flow.
