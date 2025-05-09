# Queues

A Queue is nothing more than a list of jobs waiting to be processed. The jobs can be small, message like, so that the queue can be used as a message broker, or they can be larger long running jobs.

Queues are controlled with the `Queue` class. As all classes in BullMQ, this is a lightweight class with a handful of methods that gives you control over the queue:

```typescript
const queue = new Queue('Cars');
```

{% hint style="info" %}
See [Connections](../connections.md) for details on how to pass Redis details to use by the queue.
{% endhint %}

When you instantiate a Queue, BullMQ will just _upsert_ a small "meta-key", so if the queue existed before it will just pick it up and you can continue adding jobs to it.

The most important method is probably the [_**add**_](https://api.docs.bullmq.io/classes/v5.Queue.html#add) method. This method allows you to add jobs to the queue in different fashions:

```typescript
await queue.add('paint', { color: 'red' });
```

The code above will add a job named _paint_ to the queue, with payload `{ color: 'red' }`. This job will now be stored in Redis in a list waiting for some worker to pick it up and process it. Workers may not be running when you add the job, however as soon as one worker is connected to the queue it will pick the job and process it.

When adding a job you can also specify an options object. This options object can dramatically change the behaviour of the added jobs. For example you can add a job that is delayed:

```typescript
await queue.add('paint', { color: 'blue' }, { delay: 5000 });
```

The job will now wait **at** **least** 5 seconds before it is processed.

{% hint style="danger" %}
Prior to BullMQ 2.0, in order for delay jobs to work you need to have at least one `QueueScheduler` somewhere in your infrastructure. Read more [here](../queuescheduler.md).

From BullMQ 2.0 and onwards, the `QueueScheduler` is not needed anymore.
{% endhint %}

There are many other options available such as priorities, backoff settings, lifo behaviour, remove-on-complete policies, etc. Please check the remainder of this guide for more information regarding these options.

## Read more:

- 💡 [Queue API Reference](https://api.docs.bullmq.io/classes/v5.Queue.html)
