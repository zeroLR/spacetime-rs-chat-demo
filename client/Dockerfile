# Multi-stage build for Rust client
# Build stage - Using a specific version that supports the current codebase
FROM rust:1.84-slim AS builder

# Install build dependencies
RUN apt-get update && apt-get install -y \
    pkg-config \
    libssl-dev \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Copy dependency files first for better layer caching
COPY Cargo.toml Cargo.lock ./

# Copy source code
COPY src ./src

# Note: The project uses edition 2024 which requires a newer Rust version
# If you encounter edition 2024 errors, you may need to:
# 1. Use rust:nightly image, or 
# 2. Temporarily modify edition to "2021" for stable builds
# 3. Use rust:1.84+ when it becomes available with edition 2024 support

# Build the application in release mode
RUN cargo build --release

# Runtime stage
FROM debian:bookworm-slim AS runtime

# Install runtime dependencies
RUN apt-get update && apt-get install -y \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Create a non-root user
RUN useradd -r -u 1000 -m -d /app -s /bin/bash -g root -G sudo appuser

# Set working directory
WORKDIR /app

# Copy the built binary from the builder stage
COPY --from=builder /app/target/release/client /app/client

# Set ownership and permissions
RUN chown appuser:root /app/client && chmod 755 /app/client

# Switch to non-root user
USER appuser

# Set the entrypoint to the binary
ENTRYPOINT ["/app/client"]