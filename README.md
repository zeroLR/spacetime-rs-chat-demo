# SpacetimeDB Rust Chat Demo

It is a simple chat application that allows users to send messages to each other.
The chat server is written in Rust and uses SpacetimeDB to manage the state of the chat room.

## Getting Started

To use the spacetime cli, you need to install it first, go to https://spacetimedb.com/install

To run the rust client, you need to have Rust installed. You can install Rust by following the instructions at https://www.rust-lang.org/tools/install

### Running the self-hosted server

Using spacetime cli

```bash
spacetime start -l 0.0.0.0:3000
```

Using docker

```bash
docker run --rm --pull always -p 3000:3000 --name spacetimedb clockworklabs/spacetime start
```

Check the spacetime server list by running the following command:

```bash
spacetime server list

------
 DEFAULT  HOSTNAME                   PROTOCOL  NICKNAME
     ***  127.0.0.1:3000             http      local
```

You can add more servers by running the following command:

```bash
spacetime server add --url http://localhost:3001 <NICKNAME>
```

### Publish the module to the server

```bash
spacetime publish --project-path server <MODULE_NAME>
```

### Running the Rust client

Replace the `.env.example` file with `.env` and update the environment variables with the spacetime server details.

```bash
cd client
cargo run
```

## Reference

- https://spacetimedb.com/docs/modules/rust/quickstart
- https://spacetimedb.com/docs/sdks/rust/quickstart
