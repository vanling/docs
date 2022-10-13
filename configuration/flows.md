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

Oversimplification of the internet:
- Store data
- Get Data
- Process Data
- Send Data
Database data stored as rows and columns
Web Request -> JSON
JSON -> Operations and Transformations
Scripts ->
Control Flow ->
Events ->
Async/Sync ->

### JSON vs JavaScript Objects

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

<video title="What's a Flow" autoplay playsinline muted loop controls>
<source src="https://cdn.directus.io/docs/v9/" type="video/mp4" />
</video>

Each flow is made up of three elements: A trigger, its operations, and a Flow Object.

Each Flow begins with a [Trigger](/configuration/flows/triggers), which defines the action or event that starts the
Flow. This action or event could be some type of transaction within the app, an incoming webhook, a cron job, etc.

An [operation](/configuration/flows/operations) is an action or process performed within the flow. These enable you to
create, read, update, or delete data; send off emails, push _in-app_ notifications, and log to console; create control
flows with conditional logic, send webhooks, implement your own custom scripts using JavaScript, _and beyond_.

But, to put these explicit actions in more general terms, operations do three things:

- **Get data** from Directus or an outside service.
- **Process data** a.k.a. transform it, validate it, or whatever.
- **Send data** to Directus or an outside service.

In order to track and access its data, each flow creates its own [Flow Object](#the-flow-object). Every operation has
access to this Flow Object. This means you can use data from a previous operation in the current operation, enabling you
to configure compound, complex, conditional flows for task automation and data processing.

Not every operation that executes in a flow does so successfully. In some cases, your operations are going to fail.
Perhaps an operation tried to access data that doesn't exist, or a webhook operation fails for some reason, or perhaps
you set a [Condition Operation](/configuration/flows/operations.md#condition) which _fails by design_ when its condition
is not met.

This does not immediately stop your flow and throw an error. It provides
[control flow](https://en.wikipedia.org/wiki/Control_flow). You can set up divergent chains of operations within a flow:

![Control Flow](image.webp)

- If `operation1` executes successfully, then `operation2` executes.
- Else if `operation1` fails on execution, then `operation3` executes.

_And there we have it!_ These are the conceptual cornerstones of any flow. Next, you'll need to know how to actually
create a flow, which we discuss in the next section.

## Configure a Flow

<video autoplay playsinline muted loop controls title="Create a Flow">
	<source src="https://cdn.directus.io/docs/v9/configuration/flows/flows/flows-20220603A/create-a-flow-20220603A.mp4" type="video/mp4" />
</video>

### Create A Flow

1. Navigate to **Settings > Flows** and click <span mi btn>add</span> in the page header. A drawer will open.
2. Under **Flow Setup**, fill in a **Name** for the Flow and the following _optional_ details:
   - **Status** — Sets the Flow to active or inactive.
   - **Icon** — Adds an icon to help quickly identify the Flow.
   - **Description** — Sets a brief verbal description of the Flow.
   - **Color** — Sets a color to help identify the Flow.
   - **Activity and Logs Tracking** - Track Activity and [Logs](#logs), Activity, or neither.

### Configure a Trigger

3. Click <span mi btn>arrow_forward</span> to navigate to **Trigger Setup**. Select a
   [Trigger](/configuration/flows/triggers) type and configure as desired.
4. Click <span mi btn>done</span> in the menu header to confirm.

### Configure an Operation

5. On the trigger panel, click <span mi>add</span> and the **Create Operation** side drawer will open.
6. Choose a **Name**, an [Operation](/configuration/flows/operations) type, and configure as desired.\
   Directus will convert the unique name into an operation key, used on the [Flow Object](#the-flow-object).\
   If you don't choose a name, the system will auto-generate a name and key for you.
7. Next, click <span mi btn>done</span> in the Page Header to confirm and return to the flow grid area.
8. From here, you can make the following optional configurations:
   - **Reposition** — You can drag and drop panels to reposition as desired.
   - **Unlink/Relink** — Click and drag <span mi icon prmry>adjust</span> or <span mi icon prmry>arrow_forward</span> to
     unlink/relink flows.
   - **Duplicate an Operation** —
   - **Copy an Operation** — To copy and paste an operation into another flow, click <span mi icon>more_vert</span> to
     open its context menu. Click <span mi icon>input</span> and a popup menu will open. Choose the desired flow from
     the dropdown and click **Copy**.
   - **Delete an Operation** — To delete an Operation, click <span mi icon>more_vert</span> then
     <span mi icon dngr>delete</span>. A popup menu will appear. Click <span mi icon dngr>delete</span> to confirm.
9. On the newly created Operation Panel:
   - Click <span mi icon>add</span> to add an Operation to execute if the current Operation is successful.
   - Click <span mi icon>remove</span> to add an Operation to execute if the current Operation fails.
10. Repeat steps 5-10 to build out your Flow as desired.
11. Click <span mi btn>done</span> to confirm and create your Flow.
12. Click <span mi btn>arrow_back</span> to return to the flows list.
13. Once created, you may need to reconfigure your flow, toggle it to inactive, or delete it.

### Reconfigure a Flow

1. Navigate tot the desired flow.
2. Click <span mi btn muted>edit</span> in the flow page header and make reconfigurations as desired.
3. Click <span mi btn>done</span> to confirm.

### Toggle a Flow to Inactive

1. Navigate to **Settings Flows** and click <span mi icon>more_vert</span> on the desired flow.
2. Click **<span mi icon>check</span> Set Flow to Active** or **<span mi icon>block</span> Set Flow to Inactive**.

### Delete a Flow

1. Click <span mi icon>more_vert</span> on the desired flow to open its context menu.
2. Click <span mi icon dngr>delete</span> and a popup menu will appear. Click **Delete** to confirm.

Now that we know how to create and configure a flow, it's time to get a firmer understanding of the Flow Object.

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
lot of data. However, the data nested in these keys will always be slightly different, based on your flow's unique
configuration. During configuration and debugging, you'll need to use a tool like [The Logger](#logs) to view your
flow's unique data.

:::tip

In our examples, we are using generic _placeholders_ for operation keys, like `<operationKey>`, which might look funny
to low-code users. In practice, operation keys will actually have a unique and descriptive names, like
`send_email_7538`.

:::

## Mustache Syntax

<video title="Use Flow Object Keys as Variables" autoplay playsinline muted loop controls>
<source src="https://cdn.directus.io/docs/v9/" type="video/mp4" />
</video>

While configuring your flow's operations, you can use keys from the Flow Obejct as variables to access data. Simply wrap
the key name in double-moustache syntax to access its stored value.

```
{{ $accountability }}
```

<!--
:::v-pre

#### `{{ opKey }}` vs `"{{ opKey }}"`

:::

The flows system parses the vars into JSON, then converts this to a JavaScript object to send to the API.
The problem is, some of your data might not always be valid JSON.
When you need to access a value that would not be valid JSON, you can first wrap it in "{{}}".
By doing this, it will signal to Flows that your value is an object.

For example.

- Code snippets go here.
-->

You can use dot-notation and array indexing to retrieve sub-nested values.

```
{
	"key1": "{{ $trigger.payload }}",
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

Accessible from the sidebar, logs store information for each flow execution. Each log will display information from
triggers as well as each operation in the flow. To access a flow's logs, follow these steps.

1. Navigate to **Settings > Flows** and click the desired flow.
2. Click **<span mi icon prmry>fact_check</span> Logs** in the sidebar. A side drawer will open, displaying the flow's
   logs.
3. Click a log and another side drawer will open, allowing you to peer through its data.
4. When finished, click <span mi btn muted>close</span> to close the drawer.

The Logger is not a 1:1 mapping to the Flow Object.

**Trigger**

- **Options** — Displays the trigger's configuration details as JSON.\
  _(This data is not stored on the Flow Object)_.
- **Payload** — Displays the data appended under `$trigger`.
- **Accountability** — Displays data appended under `$accountability`.

**`<OperationKey>`**

- **Options** — Displays the operation's configuration details as JSON.\
  _(This data is not stored on the Flow Object)_.
- **Payload** — Displays the data appended under this `<operationKey>`.

**`<OperationKey>` Log to Console**

- **Options** — Displays the message you used to

[Log to Console](/configuration//flows/operations.md#log-to-console) is an operation that lets you log any value or
message in The Logger, for debugging purposes. It accepts variables from the Flow Object. Your logged message is not
appended onto the Flow Object, so when using The Logger, so you can find your log under **Options**.

Note that this also means your original message will always be wrapped in JSON. So if you wanted to log a value, for
example `"The last operation was a success"`, it will be displayed as:

```
{
	"message": "The last operation was a success"
}
```

The `message` key is not part of the original message.

:::tip

Remember, some operations _(such as Log to Console)_ do not generate data. These operations will not have a **Payload**
in the Logger.

:::

:::tip Where is `$last`?

You may notice `$last` is not in The Logger. Remember, its value constantly changes, updating after every operation. To
find out the value of `$last` at any point in the flow, use
[Log to Console](/configuration/flows/operations.md#log-to-console).

:::

:::tip

In more complex flows, a tool like [Postman](https://www.postman.com/) is quite helpful to help view data and debug your
flow.

:::
