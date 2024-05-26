---
title: What composes a Rust graphql server
date: 2024-01-05
---

For a rust graphql server, I need to understand what is required to have it.
I've read about the following:

- https://graphql-rust.github.io/ juniper. Seems to be the go-to graphql server
  in rust. They link to several web server libraries.
- https://async-graphql.github.io/async-graphql/en/introduction.html
  async-graphql. Some use it over juniper. Can merge objects etc.
- https://tokio.rs/ tokio. An underlying library that powers much of the rust
  ecosystem. Allows for easier asynchronous operations and thread management (I
  think?)
- https://github.com/hyperium/tonic tonic. A "gRPC"? Whatever that is. Something
  on top of tokio, that replaces tokio-grpc, I think.
- https://hyper.rs/guides/1/server/hello-world/ hyper. Seems to be the go-to,
  most stable, most developed server library?
- https://rocket.rs/ rocket. Another server library. Uses annotations, which I
  don't like.
- https://github.com/seanmonstar/warp warp. Built on top of Hyper. Seems to add
  a lot of niceness?
- https://github.com/actix/examples/blob/master/graphql/juniper/src/main.rs#L26
  actix. Uses annotations.
- https://github.com/poem-web/poem poem. Another server implementation. Uses
  annotations, but less so. Seems pretty nice?
- https://github.com/tokio-rs/axum doesn't use annotations. Builds on top of
  hyper, tokio and tower.
