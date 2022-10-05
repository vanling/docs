---
description:
  Flows enable custom, event-driven data processing and task automation within Directus. Each Flow is composed of one
  Trigger, followed by a series of Operations.
readTime: 5 min read
---

# Flows

> Flows enable custom, event-driven data processing and task automation within Directus. Each Flow is composed of one
> Trigger, followed by a series of Operations.

:::tip Before You Begin

Please be sure to see the [Quickstart Guide](/getting-started/quickstart.md) to get a basic overview of the platform.

:::

:::tip Learn More

There is also dedicated API documentation on [Flows](/reference/system/flows) and
[Operations](/reference/system/operations).

:::

## What's a Flow?

![What's a Flow?](https://cdn.directus.io/docs/v9/configuration/flows/flows/flows-20220603A/whats-a-flow-20220603A.webp)

Each flow is made up of three elements: A trigger, the operations, and the Flow Object.

Each Flow begins with a [Trigger](/configuration/flows/triggers), which defines the action or event that starts the
Flow. This action or event could be some type of transaction within the app, an incoming webhook, a cron job, etc.

[Operations](/configuration/flows/operations) are the actions performed within the flow. These enable you to create,
read, update, or delete data, send off emails, push _in-app_ notifications, send webhooks, write your own custom scripts
using JavaScript/TypeScript, _and beyond_.

In general, operations basically do three things:

- Fetch data (from Directus or an outside service).
- Process data (transform it, validate it, or whatever).
- Send data (to Directus or an outside service).

To track and access data, each flow creates its own [Flow Object](#the-flow-object). This is a JSON object that stores
data generated from the Trigger as well as each operation that executes in the flow. All generated data is appended onto
the Flow Object. Every Operation has access to the Flow Object. Therefore, you can chain multiple operations together to
do create compound, complex, conditional task automation and data processing.

Not every operation that executes in a flow does so successfully. In some cases, your operations are going to fail.
Perhaps an operation tried to access data that doesn't exist, or a webhook operation fails for some reason, or perhaps
you set a [Condition Operation](/configuration/flows/operations.md#condition) which fails when its condition is not met.

In order to provide [control flow](https://en.wikipedia.org/wiki/Control_flow) to your Flows, you can set up divergent
chains of operations.

![Control Flow](image.webp)

- If `operation1` executes successfully, then `operation2` executes.
- Else if `operation1` fails on execution, then `operation3` executes.

_And there we have it!_ These are the core building block of any flow. However, during configuration, and in the event
of an unexpected error, you'll need some way to view Flow Object data and debug your flow. So Flows also includes a
[Logger](#logs), which we'll also introduce below.

## Create a Flow

<video autoplay playsinline muted loop controls title="Create a Flow">
	<source src="https://cdn.directus.io/docs/v9/configuration/flows/flows/flows-20220603A/create-a-flow-20220603A.mp4" type="video/mp4" />
</video>

To create a Flow, follow these steps:

1. Navigate to **Settings > Flows** and click <span mi btn>add</span> in the page header. A drawer will open.
2. Under **Flow Setup**, fill in a **Name** for the Flow and the following _optional_ details:
   - **Status** — Sets the Flow to active or inactive.
   - **Icon** — Adds a Material Icon used to help quickly identify the Flow.
   - **Description** — Sets a brief verbal description of the Flow.
   - **Color** — Sets a color to help identify the Flow.
   - **Activity and Logs Tracking** - Track Activity and [Logs](#logs), Activity, or neither.
3. Click <span mi btn>arrow_forward</span> to navigate to **Trigger Setup**. Select a
   [Trigger](/configuration/flows/triggers) type and configure as desired.
4. Click <span mi btn>done</span> in the Menu Header to be taken to the Flow Grid Area.\
   You will now see the Trigger Panel on the Flow Grid Area.
5. On the Trigger Panel, click <span mi>add</span> and the **Create Operation** side menu will open.
6. Choose a **Name**, an [Operation](/configuration/flows/operations) type, and configure as desired.\
   Directus will convert the unique name into an Operation Key, used on the [Flow Object](#the-flow-object).\
   If you don't choose a name, the system will auto-generate a name and key.
7. Next, click <span mi btn>done</span> in the Page Header to confirm and return to the Flow Grid Area.
8. On the newly created Operation Panel:
   - Click <span mi icon>add</span> to add an Operation to execute if the current Operation is successful.
   - Click <span mi icon>remove</span> to add an Operation to execute if the current Operation fails.
9. Repeat steps 8-10 to build out your Flow as desired.
10. **Optional:** To reconfigure a Trigger or Operation Panel, click <span mi icon>edit</span> and make any necessary
    edits.
11. **Optional:** To delete an Operation Panel, click <span mi icon>more_vert</span> then
    <span mi icon dngr>delete</span>.
12. **Optional:** To unlink an operation, click <span mi icon prmry>adjust</span> and drag.

<!--
Create a Flow
View a Flow
Edit a Flow
Toggle a Flow to (In)active
Delete a Flow

Configure a Trigger

Create an Operation
Configure an Operation
(Un)link an Operation
The Context Menu
Delete an Operation
-->

## The Flow Object

<!--
Video exploration of the Flow Object
What's the difference? "{{}}" vs {{}}


-->

When a Flow is triggered, Directus creates a JSON object to store all data generated within the flow's trigger and
operation(s). Every operation has access to this Flow Object, and therefore data stored from preceding operations.

As each operation executes successfully, an `operation_key` is appended to the Flow Object. Any data was generated
during the operation is stored under its relevant `operation_key`. If the operation doesn't generate data, a `null`
value is appended under its key.

The following is a generic example of a Flow Object.

```json
{
	"$trigger": {}, // Data generated by the Flow trigger.
	"$last": null, // Constantly updates to store data of the last operation that executed in the flow.
	"$accountability": {}, // Provides details on who/what started the flow.
	"operationKey1": "some_value", // The data (if any) generated by the operation.
	"operationKey2": {
		"nestedKey": "Nested value" // It will be common to have nested JSON.
	},
	"operationKey3": null, // A null value is appended if no data generated.
	...
}
```

### JSON

As you [create a flow](#create-a-flow), remember that the Flow Object is a JSON object. So all keys and values must
follow [JSON syntax](https://www.w3schools.com/js/js_json_syntax.asp). In general, this means each `key` must be a
string, which can contain _but cannot begin with_ a number. Each `value` can be of the following type:

```json
{
	"key": "string", // string
	"key2": 1, // number
	"key3": {}, //json
	"key4": [], // array
	"key5": true, // boolean
	"key6": null // null
}
```

### Flow Object Variables

Each Flow Object comes with the following keys:

- `$all` — This stores the entire Flow Object.
- `$trigger` — This stores data generated from the trigger.
- `$last` — When configuring operation, use `$last` in the current operation to conveniently access data from the
  preceding operation, without needing to lookup its specific `operationKey`
- `$accountability` — Provides details on who/what started the flow.
- `<operationKey>` — All the operation keys.

### Dot Notation

You can use double-moustache syntax to access an existing value on the Flow Object.

```json
{
	"operationKey4": {
		"user": "{{ $accountability.user }}"
	}
}
```

These variables are not a feature of JSON. This is powered by Directus, _under the hood_.

### Indexing

You can use dot-notation and array indexing.

```json
{
	"key1": {{ $trigger.payload }}, // inserts value nested under $trigger.payload
	"key2": {{ }},
}
```

You cannot pass any type of computation using double-moustache syntax:

```json
{
	"key": {{ 2 + 2 }}, // doesn't work
	"key2": {{ $trigger.payload.toLowerCase() }} // doesn't work
}
```

:::tip

To perform computations, use the [script operation](/configuration/flows/operations.md#script) or a
[webhook](/configuration/flows/operations.md#webhook).

:::

## Logs

<!--
Options
 -->

<video autoplay playsinline muted loop controls title="">
	<source src="https://cdn.directus.io/docs/v9/configuration/flows/flows/flows-20220603A/logs-20220603A.mp4" type="video/mp4" />
</video>

Accessible from the Sidebar, Logs store information for each Flow execution. Each log will display information from
Triggers as well as each Operation in the Flow. To access a Flow's logs, follow these steps.

1. Navigate to **Settings > Flows** and click the desired Flow.
2. Click **<span mi icon prmry>fact_check</span> Logs** in the Sidebar. A side drawer will open, displaying the Flow's
   logs.
3. Click a log and another side drawer will open, allowing you to peer through its data.
4. When finished, click <span mi btn muted>close</span> to close the drawer.
