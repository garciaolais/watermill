# Watermill
<img align="right" width="200" src="https://threedots.tech/watermill-io/watermill-logo.png">

[![CircleCI](https://circleci.com/gh/ThreeDotsLabs/watermill/tree/master.svg?style=svg)](https://circleci.com/gh/ThreeDotsLabs/watermill/tree/master)
[![Go Report Card](https://goreportcard.com/badge/github.com/ThreeDotsLabs/watermill)](https://goreportcard.com/report/github.com/ThreeDotsLabs/watermill)
[![codecov](https://codecov.io/gh/ThreeDotsLabs/watermill/branch/master/graph/badge.svg)](https://codecov.io/gh/ThreeDotsLabs/watermill)

Watermill is a Go library for working efficiently with message streams. It is intended
for building event driven applications, enabling event sourcing, RPC over messages,
sagas and basically whatever else comes to your mind. You can use conventional pub/sub
implementations like Kafka or RabbitMQ, but also HTTP or MySQL binlog if that fits your use case.

Documentation: https://watermill.io/

Getting started guide: https://watermill.io/docs/getting-started/

## Background

Building distributed and scalable services is rarely as easy as some may suggest. There is a
lot of hidden knowledge that comes with writing such systems. Just like you don't need to know the
whole TCP stack to create a HTTP REST server, you shouldn't need to study all of this knowledge to
start with building message-driven applications.

Watermill's goal is to make communication with messages as easy to use as HTTP routers. It provides
the tools needed to begin working with event-driven architecture and allows you to learn the details
on the go.

At the heart of Watermill there is one simple interface:
```go
func(*Message) ([]*Message, error)
```

Your handler receives a message and decides whether to publish new message(s) or return
an error. What happens next is up to the middlewares you've chosen.

You can find more about our motivations in our [*Introducing Watermill* blog post](https://threedots.tech/post/introducing-watermill/).

## Features

* **Easy** to understand (see examples below).
* **Universal** - event-driven architecture, messaging, stream processing, CQRS - use it for whatever you need.
* **Fast** - *(benchmarks coming soon)*
* **Flexible** with middlewares and plugins.
* **Resilient** - using proven technologies and passing stress tests *(results coming soon)*.

## Pub/Subs

All publishers and subscribers have to implement an interface:

```go
type Publisher interface {
	Publish(topic string, messages ...*Message) error
	Close() error
}

type Subscriber interface {
	Subscribe(ctx context.Context, topic string) (<-chan *Message, error)
	Close() error
}
```

Supported Pub/Subs:


- AMQP Pub/Sub [(`github.com/ThreeDotsLabs/watermill-amqp`)](https://github.com/ThreeDotsLabs/watermill-amqp/)
- Google Cloud Pub/Sub [(`github.com/ThreeDotsLabs/watermill-googlecloud`)](https://github.com/ThreeDotsLabs/watermill-googlecloud/)
- HTTP Pub/Sub [(`github.com/ThreeDotsLabs/watermill-http`)](https://github.com/ThreeDotsLabs/watermill-http/)
- io.Reader/io.Writer Pub/Sub [(`github.com/ThreeDotsLabs/watermill-io`)](https://github.com/ThreeDotsLabs/watermill-io/)
- Kafka Pub/Sub [(`github.com/ThreeDotsLabs/watermill-kafka`)](https://github.com/ThreeDotsLabs/watermill-kafka/)
- NATS Pub/Sub [(`github.com/ThreeDotsLabs/watermill-nats`)](https://github.com/ThreeDotsLabs/watermill-nats/)
- SQL Pub/Sub [(`github.com/ThreeDotsLabs/watermill-sql`)](https://github.com/ThreeDotsLabs/watermill-sql/)


All Pub/Subs implementation documentation can be found in the [documentation](https://watermill.io/docs/pub-sub-implementations/).

## Examples
* [Your first app](_examples/your-first-app) - start here!
* [Simple application with publisher and subscriber](_examples/simple-app)
* [HTTP to Kafka](_examples/http-to-kafka)
* [NATS example](https://github.com/ThreeDotsLabs/nats-example)
* [RabbitMQ, webhooks and Kafka integration](https://github.com/ThreeDotsLabs/event-driven-example)
* [...and more!](_examples/)

## Contributing

Please check our [contributing guide](CONTRIBUTING.md).

## Stability

Watermill v1.0.0 has been released and is production-ready. The public API is stable and will not change without changing the major version.

To ensure that all Pub/Subs are stable and safe to use in production, we created a [set of tests](https://github.com/ThreeDotsLabs/watermill/blob/master/pubsub/tests/test_pubsub.go#L34) that need to pass for each of the implementations before merging to master.
All tests are also executed in [*stress*](https://github.com/ThreeDotsLabs/watermill/blob/master/pubsub/tests/test_pubsub.go#L171) mode - that means that we are running all the tests **20x** in parallel.

All tests are run with the race condition detector enabled (`-race` flag in tests).

## Support

If you didn't find the answer to your question in [the documentation](https://watermill.io/), feel free to ask us directly!

Please join us on the `#watermill` channel on the [Gophers slack](https://gophers.slack.com/): You can get an invite [here](https://gophersinvite.herokuapp.com/).

Every bit of feedback is very welcome and appreciated. Please submit it using [the survey](https://www.surveymonkey.com/r/WZXD392).

## Why the name?

It processes streams!

## License

[MIT License](./LICENSE)
