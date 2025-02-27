# Get topic messages

Subscribe to a topic ID's messages from a mirror node. You will receive all messages for the specified topic or within the defined start and end time.

**Query Fees**

* Please see the transaction and query [fees](../../../mainnet/fees/#transaction-and-query-fees) table for base transaction fee
* Please use the [Hedera fee estimator](https://hedera.com/fees) to estimate your query fee cost

{% tabs %}
{% tab title="V2" %}
| Constructor               | Description                              |
| ------------------------- | ---------------------------------------- |
| `new TopicMessageQuery()` | Initializes the TopicMessageQuery object |

```java
new TopicMessageQuery()
```

## Methods

| Method                       | Type               | Description                                         | Requirement |
| ---------------------------- | ------------------ | --------------------------------------------------- | ----------- |
| `setTopicId(<topicId>)`      | TopicId            | The topic ID to subscribe to                        | Required    |
| `setStartTime(<startTime>)`  | Instant            | The time to start subscribing to a topic's messages | Optional    |
| `setEndTime(<endTime>)`      | Instant            | The time to stop subscribing to a topic's messages  | Optional    |
| `setLimit(<limit>)`          | long               | The number of messages to return                    | Optional    |
| `subscribe(<client, onNext)` | SubscriptionHandle | Client, Consumer\<TopicMessage>                     | Required    |

{% code title="Java" %}
```java
//Create the query
new TopicMessageQuery()
    .setTopicId(newTopicId)
    .subscribe(client, topicMessage -> {
        System.out.println("at " + topicMessage.consensusTimestamp + " ( seq = " + topicMessage.sequenceNumber + " ) received topic message of " + topicMessage.contents.length + " bytes");
    });

//v2.0.0
```
{% endcode %}

{% code title="JavaScript" %}
```javascript
//Create the query
new TopicMessageQuery()
        .setTopicId(topicId)
        .setStartTime(0)
        .subscribe(
            client,
            (message) => console.log(Buffer.from(message.contents, "utf8").toString())
        );
//v2.0.0
```
{% endcode %}

{% code title="Go" %}
```java
//Create the query
_, err = hedera.NewTopicMessageQuery().
    SetTopicID(topicID).
    Subscribe(client, func(message hedera.TopicMessage) {
        if string(message.Contents) == content {
        wait = false
    }
})

if err != nil {
    panic(err)
}

//v2.0.0
```
{% endcode %}
{% endtab %}

{% tab title="V1" %}
| Constructor                       | Description                                      |
| --------------------------------- | ------------------------------------------------ |
| `new MirrorConsensusTopicQuery()` | Initializes the MirrorConsensusTopicQuery object |

```java
new MirrorConsensusTopicQuery()
```

## Methods

| Method                                     | Type                                                                         | Description                                         | Requirement |
| ------------------------------------------ | ---------------------------------------------------------------------------- | --------------------------------------------------- | ----------- |
| `setTopicId(<topicId>)`                    | TopicId                                                                      | The topic ID to subscribe to                        | Required    |
| `setStartTime(<startTime>)`                | Instant                                                                      | The time to start subscribing to a topic's messages | Optional    |
| `setEndTime(<endTime>)`                    | Instant                                                                      | The time to stop subscribing to a topic's messages  | Optional    |
| `setLimit(<limit>)`                        | long                                                                         | The number of messages to return                    | Optional    |
| `subscribe(<mirrorClient, onNext onError)` | MirrorClient, Consumer \<MirrorConsensusTopicResponse>, Consumer\<Throwable> | Subscribe and get the  messages for a topic         | Required    |

{% code title="Java" %}
```java
new MirrorConsensusTopicQuery()
    .setTopicId(topicId)
    .subscribe(mirrorClient, resp -> {
                String messageAsString = new String(resp.message, StandardCharsets.UTF_8);

                System.out.println(resp.consensusTimestamp + " received topic message: " + messageAsString);
            },
            // On gRPC error, print the stack trace
            Throwable::printStackTrace);
//v1.3.2
```
{% endcode %}

{% code title="JavaScript" %}
```javascript
new MirrorConsensusTopicQuery()
    .setTopicId(topicId)
    .subscribe(
        consensusClient,
        (message) => console.log(message.toString()),
        (error) => console.log(`Error: ${error}`)
    );
```
{% endcode %}
{% endtab %}
{% endtabs %}
