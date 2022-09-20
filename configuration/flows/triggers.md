---
description:
  A Trigger initiates a Flow on some internal or external event, such as an in-app activity, incoming webhook, cron job,
  execution of Operation(s) from other Flows, and beyond!
readTime: 3 min read
---

# Triggers

> A Trigger defines the event that starts your Flow. The event could be form an internal or external activity, such as
> changes to data, logins, errors, incoming webhook, cron jobs, execution of Operation(s) from other Flows, _and
> beyond!_

:::tip Before You Begin

Please be sure to read the documentation on [Flows](/configuration/flows).

:::

## Event Hook

![Event Hooks](https://cdn.directus.io/docs/v9/configuration/flows/triggers/triggers-20220603A/event-hook-20220602A.webp)

Event Hooks are triggered by platform or data events. The logic is based on [Custom API Hooks](/extensions/hooks). If
your event generates data, this will be stored in the Flow Object.

- **Type** — Choose the type of Event Hook:
  - **Filter (Blocking)** — Fires "blocking" right before the database transaction is committed, allowing you to tweak
    the payload or prevent completion outright.
  - **Action (Non-Blocking)** — Does not block anything. A non-blocking action is mostly useful for completing tasks in
    response to an event, without slowing down the API.
- **Scope** — Set the types of events that trip this Trigger.
- **Collections** — Set the Collections that trip this Trigger.
- **Response Body** — This is optional and only for Filter (Blocking) events. Returns Flow Object data to the event,
  overwriting its original payload. Choose to return:
  - **Data of last Operation** — Same as `$last`.
  - **All Data** — Same as `$all`
  - **Other** — Another operation key from the flow.

::: tip Multiple Items

Only one item is passed into each flow. For example, if you create, update or delete multiple items in a single request,
the Flow will trigger once for each and every item. If you create three items, the flow is triggered three times, once
for each item.

:::

### Filters vs Actions

![Blocking Events](image.webp)

A Filter is a blocking event trigger. This means it will pause the event, force the whole flow to run, then commit the
transaction to the database.

For example, let's say you configure the scope to be `item.create` on some collection(s). When you send a request create
items on the relevant collection(s):

- A request to create an item is sent to Directus.
- The create item event pauses.
- The flow's trigger is tripped.
- The event's payload passes into the `$trigger` of the flow.
- The flow runs.
- **Optional:** If you defined a **Response Body**, this replaces the event's payload.
- The transaction is committed to the database (i.e., the item is created).

Since the event is paused until the end of the flow, since you have access to the original payload within `$trigger`,
and since you can overwrite the original payload with **Response Body**, Filters enable you to validate or transform the
payload before it is committed to the database.

![Non-Blocking Events](image.webp)

An Action is a non-blocking event trigger. This lets the event commit its transaction to the database without waiting
for the flow to finish. Therefore, Actions give you a more performant API, but you have no ability to validate or
transform the payload before the transaction is committed to the database.

## Webhook

![Webhook](https://cdn.directus.io/docs/v9/configuration/flows/triggers/triggers-20220603A/webhook-20220602A.webp)

Triggers on an incoming HTTP request to: `/flows/trigger/:this-webhook-trigger-id` which you can copy from the webhook
trigger panel.

- **Method** — Choose whether the flow will be triggered by a `GET` or `POST` request from the dropdown.
- **Asynchronous** — Toggle whether or not the Trigger responds asynchronously.
- **Response Body** — Returns Flow Object data as a response body to the incoming HTTP request. Choose to return:
  - **Data of last Operation** — Same as `$last`.
  - **All Data** — Same as `$all`.
  - **Other** — Another operation key from the flow.

## Schedule

![Schedule a Cron Job](https://cdn.directus.io/docs/v9/configuration/flows/triggers/triggers-20220603A/cron-20220602A.webp)

This Trigger enables you to create Data at scheduled intervals, via 6-point cron job syntax.

- **Interval** — Set the cron job interval to schedule when the Flow triggers.

## Another Flow

![Another Flow](https://cdn.directus.io/docs/v9/configuration/flows/triggers/triggers-20220603A/another-flow-20220602A.webp)

This Trigger executes a Flow via the [Trigger Flow](/configuration/flows/operations#another-flow) Operation, allowing
you to chain Flows together.

- **Response Body** — Select data to return in the response to the Trigger Flow.

## Manual

![Manual](https://cdn.directus.io/docs/v9/configuration/flows/triggers/triggers-20220603A/manual-20220602A.webp)

This Trigger starts your Flow on a manual click of a button within the Directus App. When you use this Trigger, a
**Flows** menu containing a button will appear in the Sidebar of the specified Collection Page(s) and/or Item Pages,
based on your **Location** configuration.

- **Collections** — Choose the Collection(s) to add the Button to.
- **Location** — Choose to add the button into the Item Page, Collection Page, or both.
- **Asynchronous** — Toggle whether or not the Flow executes asynchronously.
