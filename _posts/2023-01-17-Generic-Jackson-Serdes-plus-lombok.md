---
layout: post
title:  "Generic Jackson Serde (Serlializer - Deserializer) with Lombok for Kafka Streams "
date:   2023-01-17 16:56:37 +0100
tags: jackson java kafka
language: EN
---


## Motivation

While it is true that Avro is de facto standard for data serialization in Kafka, sometimes we don't want to use it for several reasons: don't want to set up the schema registry, generate the classes or write the Avro schema. As in a Proof of concept or a little project where JSON can meet our needs.

It is also true that writing your own `Serdes` is not fun at all.

I came up with a solution that satisfies me: a Generic Jackson Serdes to use for any Data Type

{% highlight java  %}
 new JacksonSerdes<>(BigDecimal.class);
{% endhighlight %}


It would be nice to make the invocation even shorter knowing the Generic class type but, [due to type erasure](https://docs.oracle.com/javase/tutorial/java/generics/erasure.html), it is not possible. 

If we make use of the [Lombok - Jakson integration](https://projectlombok.org/features/experimental/Jacksonized) for our `Data` classes we reduce boilerplate code even more.

## Data Class

{% highlight java %}
@Builder
@Jacksonized
@Data
public class Transaction {

    @JsonProperty("transaction_code")
    private final String transactionCode;
    
    private final int accountId;
    private final Date date;
    private final int amount;
} 
{% endhighlight %}

##  Kafka Serdes

{% highlight java %}
  public class JacksonSerdes<T> implements Serde<T> {

    private final Class<T> type;

    public JacksonSerdes(Class<T> type) {
        this.type = type;
    }

    @Override
    public Serializer<T> serializer() {
        return new JacksonSerializer<T>();
    }

    @Override
    public Deserializer<T> deserializer() {
        return new JacksonDeserializer<T>(type);
    }
  }
{% endhighlight %}

## Deserializer

{% highlight java %}
public class JacksonDeserializer<T> implements Deserializer<T> {

    private final ObjectMapper mapper;

    private final Class<T> type;

    public JacksonDeserializer(Class<T> type) {
        this.type = type;
        mapper = new ObjectMapper();
    }

    @Override
    public T deserialize(String topic, byte[] data) {
        try {
            return mapper.readValue(data, type);
        } catch (IOException e) {
            e.printStackTrace();
            return null;
        }
    }
{% endhighlight %}

## Serializer 

{% highlight java %}
public class JacksonSerializer <T>  implements Serializer<T> {

    private final ObjectMapper mapper = new ObjectMapper();

    @Override
    public byte[] serialize(String topic, T data) {
        try {
            return mapper.writeValueAsBytes(data);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
            return null;
        }
    }    
{% endhighlight %}

