title: actix

# Actix - Microframework using Actor Pattern

Actix is a concept of an actor system for Rust to be also used for web systems with `actix-web`. It's a microframework which is easy to use.

Actix is similar to what you might see in a language like Erlang, except that it adds another degree of robustness and speed by making heavy use of Rust's sophisticated type and concurrency systems.
For example, it's not possible for an actor to receive a message that it can't handle at runtime because it would have been disallowed at compile-time.

The following description is mostly an abstract of the crates [Actix QuickStart](https://actix.rs/book/actix/) documentation.

## Actor Model

In the actor model everything is an actor. This is similar to the everything is an object philosophy used by some object-oriented programming languages.

An actor is a computational entity that, in response to a message it receives, can concurrently:

-   make local decisions
-   change internal state
-   create new actors
-   send messages to other actors

There is no assumed sequence to the above actions and they could be carried out in parallel.
A fundamental advance of the Actor model is the decoupling between sender and receiver enabling asynchronous communication and control structures as patterns of passing messages.
Recipients are identified by address, sometimes called "mailing address". Thus an actor can only communicate with actors whose addresses it has. It can obtain those from a message it receives, or if the address is for an actor it has itself created.

It is a way to design concurrent systems. Rust purposefully does not enforce a design on concurrency as much as other languages might, but instead provides the necessary data structures and zero-cost abstractions, upon which people can build higher-level abstractions. The actor model is one of those higher-level abstractions, and as we have seen with other languages, it is a good model for designing some concurrent systems.

## Actor

Any rust type can be an actor, it only needs to implement the `Actor` trait.

To be able to handle a specific message the actor has to provide a `Handler` implementation for this message.

### Lifecycle

The actor can be in one of multiple states:

-   **Started**

    This is the initial state. The `started()` hook method is being called to start more actors or initialize something.

-   **Running**

    This state comes directly after the `started()` hook is done and will stay.

-   **Stopping**

    This state will be set if:

    -   `Context::stop` is called by the actor itself
    -   all addresses to the actor get dropped. i.e. no other actor references it
    -   no event objects are registered in the context

    Incomming messages are no longer processed and the `stopping()` hook is being called. The actor may go back to running state by creating a new address or adding an event object, and by returning `Running::Continue`. If the actor does not restore back to the running state, all unprocessed messages are dropped.

    By default this method returns `Running::Stop` which confirms the stop operation.

-   **Stopped**

    After stopping this state is considered final and at this point the actor is dropped.

### Address

Actors communicate exclusively by exchanging messages. To reference them their address has to be used.

!!! example "Async actor in same thread"

    ```rust
    struct MyActor;
    impl Actor for MyActor {
        type Context = Context<Self>;
        fn started(&mut self, ctx: &mut Context<Self>) {
            let addr = ctx.address(); // get self address
        }
    }

    let addr = MyActor.start();
    ```

## Message

Messages are used to communicate with and between actors. For actix each message has to implement the `Message` trait. `Result` defines the return type.

```rust
struct PlusOne;
impl Message for PlusOne {
    type Result = u32;
}
```

All messages go to the actor's mailbox first, then the actor's execution context calls specific message handlers.

To be able to handle a specific message the actor has to provide a `Handler<M>` implementation for this message. The message can be handled in an asynchronous fashion. The actor can spawn other actors or add futures or streams to the execution context.

To send a message to an actor, the `Addr` object needs to be used. It provides several ways to send a message:

-   `do_send(M)` - ignores the actor's mailbox capacity and puts the message to a mailbox unconditionally, it does not return the result of message handling and fails silently if the actor is gone

-   `try_send(M)` - try to send the message immediately, if the mailbox is full or closed (actor is dead), this method returns a SendError

-   `send(M)` - return a future object that resolves to a result of a message handling process

### Recipient

The recipient address can be used in case the message needs to be sent to a different type of actor. A recipient object can be created from an address with `Addr::recipient()`.

For example recipient can be used for a subscription system there actors can subscribe to get signal messages on specific events.

## Actix Web

This is a web framework layered on `actix` as very fast development framework for web based services and brings the actor model to the web.

See more at [actix-web](actix-web.md).

{!docs/abbreviations.txt!}
