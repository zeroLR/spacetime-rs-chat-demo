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

#### Local development

```bash
cd client
cargo run
```

#### Using Docker

You can also run the client using Docker:

1. **Build the Docker image:**
   ```bash
   cd client
   docker build -t spacetime-rs-client .
   ```

   *Note: If you encounter SSL certificate issues during Docker build, build the binary locally first:*
   ```bash
   cargo build --release
   # Then use the simple build approach documented in the Dockerfile
   ```

2. **Run the client container:**
   ```bash
   # Pass environment variables at runtime
   docker run --rm \
     -e HOST=http://localhost:3000 \
     -e DB_NAME=your_module_name \
     spacetime-rs-client
   
   # Or using an environment file
   docker run --rm --env-file .env spacetime-rs-client
   ```

3. **Using Docker Compose (optional):**
   Create a `docker-compose.yml` file:
   ```yaml
   version: '3.8'
   services:
     client:
       build: ./client
       environment:
         - HOST=http://localhost:3000
         - DB_NAME=your_module_name
       # Uncomment to use .env file instead
       # env_file: ./client/.env
   ```

   Then run: `docker-compose up`

## Reference

- https://spacetimedb.com/docs/modules/rust/quickstart
- https://spacetimedb.com/docs/sdks/rust/quickstart
