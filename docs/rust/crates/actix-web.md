title: actix_web

# Actix Web - Web Server Framework

This is a web framework layered on [`actix`](actix.md) as very fast development framework for web based services and brings the actor model to the web.

An application developed with `actix-web` will expose an HTTP server contained within a native executable. You can either put this behind another HTTP server like Nginx or serve it up as-is. Even in the complete absence of another HTTP server `actix-web` is powerful enough to provide HTTP 1 and HTTP 2 support as well as SSL/TLS. Also WebSockets, steaming responses, graceful shutdown, cookie support, static file serving, and good testing infrastructure are readily available out of the box.

The key facts are:

-   request handler functions which gets a request and returns a response
-   application instance which registers the request handler with a route
-   main function which starts the server

Most part of this abstract comes from the [Actix Web](https://actix.rs/docs/) documentation.

## Application

The application instance is the central element. It is used for registering routes for resources and middleware. It also stores application state shared across all handlers within same application.

The server can be run with multiple applications.

```rust
fn index(req: &HttpRequest) -> impl Responder {
    "Hello world!"
}

let app = App::new()
    .prefix("/app")
    .resource("/index.html", |r| r.method(Method::GET).f(index))
    .finish()
```

The `prefix` is used for the whole application if set. So in the above the `index` function is invoked using `/app/index.html` on the server.

{!docs/abbreviations.txt!}
