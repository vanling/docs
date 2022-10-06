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

<!--
What is Task Automation?

Oversimplification of the internet: Get Data, Process Data, Send Data
Database data stored as rows and columns
Web Request -> JSON
JSON -> Operations and Transformations
Scripts
Control Flow

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
-->

## What's a Flow?

![What's a Flow?](https://cdn.directus.io/docs/v9/configuration/flows/flows/flows-20220603A/whats-a-flow-20220603A.webp)

Each flow is made up of three elements: A trigger, its operations, and a Flow Object.

Each Flow begins with a [Trigger](/configuration/flows/triggers), which defines the action or event that starts the
Flow. This action or event could be some type of transaction within the app, an incoming webhook, a cron job, etc.

An [operation](/configuration/flows/operations) is an action or process performed within the flow. These enable you to
create, read, update, or delete data; send off emails, push _in-app_ notifications, and log to console; create control
flows with conditional logic, send webhooks, implement your own custom scripts using JavaScript, _and beyond_.

However, to put all that in more general terms, operations do three things:

- **Get data** from Directus or an outside service.
- **Process data** transform it, validate it, or whatever.
- **Send data** to Directus or an outside service.

<!--
Actually, whole purpose of flows is encapsulated by those three things: *Some action or event triggers your flow. The event data *if any* is passed in to the flow. Your operations let you get additional data, process data, then send that data off.* This is a totally open-ended system. Your flow's data could take any shape you want and go anywhere you want.
-->

In order to track and access its data, each flow creates its own [Flow Object](#the-flow-object). This is a JSON object
that stores data generated from the Trigger as well as each operation that executes in the flow. Every Operation has
access to the Flow Object. This means you can use data from a previous operation in the current operation, creating
compound, complex, conditional flows for task automation and data processing.

Not every operation that executes in a flow does so successfully. In some cases, your operations are going to fail.
Perhaps an operation tried to access data that doesn't exist, or a webhook operation fails for some reason, or perhaps
you set a [Condition Operation](/configuration/flows/operations.md#condition) which _(fails by design)_ when its
condition is not met.

This does not immediately stop your flow and throw an error. It provides
[control flow](https://en.wikipedia.org/wiki/Control_flow). You can set up divergent chains of operations within a flow:

![Control Flow](image.webp)

- If `operation1` executes successfully, then `operation2` executes.
- Else if `operation1` fails on execution, then `operation3` executes.

_And there we have it!_ These are the conceptual cornerstones of any flow. Next, you'll need to know how to actually
create a flow, which we discuss in the next section.

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

Now that we know how to create a flow, as well as how to configure its trigger and operations, it's time to get a firmer
understanding of the Flow Object.

## The Flow Object

<video title="The Flow Object" autoplay playsinline muted loop controls>
<source src="https://cdn.directus.io/docs/v9/" type="video/mp4" />
</video>

Each flow creates a JSON object to store all data generated by its trigger and operations.

When the flow begins, three keys are appended to the Flow Object: `$trigger`, `$accountability`, and `$last`. Then, as
each operation executes, it has access to the Flow Object. Once an operation finishes, its data is appended under its
`<operationKey>`. When the operation doesn't generate data, `null` is appended under its key.

The following is a generic example of a Flow Object.

```json
{
	"$trigger": {
		// Contains data generated by the Flow trigger.
		// If applicable, includes the request's body or payload.
	},
	"$accountability": {
		// Provides details on who/what started the flow such as:
		// The user's id, role, ip address, etc...
	},
	"$last": {
		// $last stores data of the last operation that executed in the flow.
		// Therefore, the value appended under $last is constantly updating.
		// This is a handy tool while configuring operations.
	},
	"operationKey1": "some_value", // The data (if any) generated by the operation.
	"operationKey2": {
		"nestedKey": "Nested value" // It will be common to have nested JSON.
	},
	"operationKey3": null, // A null value is appended if no data generated.
	...
}
```

As you can see, the example above doesn't have any substantial data inside each key. In reality, there's going to be a
lot of data. However, the data nested in these first-level keys will always be slightly different, based on your flow's
unique configuration. During configuration and debugging, you'll need to use a tool like the [Logger](#logs) to view the
data you are working with.

:::tip

In our examples, we are using generic _placeholders_ for operation keys, like `<operationKey>`, which might look funny
to low-code users. In practice, operation keys will actually have a unique and descriptive names, like
`send_email_7538`.

:::

### Use Flow Object Keys as Variables

<!--
"{{}}" versus {{}}

[
 {{ blah }}
]
-->

<video title="Use Flow Object Keys as Variables" autoplay playsinline muted loop controls>
<source src="https://cdn.directus.io/docs/v9/" type="video/mp4" />
</video>

While configuring an operation, you can use its keys as variables to access data.

While configuring an operation, you can use `$trigger`, `$accountability`, `$last`, or any preceding `<operationKey>` as
a variable. Simply wrap the keyname in double-moustache syntax to use it.

```
{{ $accountability }}
```

You can use dot-notation and array indexing.

```
{
	"key1": {{ $trigger.payload }},
	"key2": {{ $trigger.payload.friend_list[0]}}
}
```

You **cannot** pass any type of computation using double-moustache syntax:

```
{
	"key": {{ 2 + 2 }},
	"key2": {{ $trigger.payload.toLowerCase() }}
}
// These return undefined, because they aren't keys on the Flow Object.
```

:::tip

To perform computations on flow data, use the [script operation](/configuration/flows/operations.md#script) or a
[webhook](/configuration/flows/operations.md#webhook).

:::

## Logs

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

<!--
Options
-->

:::tip

In more complex flows, a tool like [Postman](https://www.postman.com/) is quite helpful to help view data and debug your
flow.

:::
