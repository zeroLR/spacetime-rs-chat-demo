# Multi-stage build for Rust client
# Build stage - for environments where network access works properly
FROM rust:1.83 AS builder

# Install build dependencies
RUN apt-get update && apt-get install -y \
    pkg-config \
    libssl-dev \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Copy source files
COPY Cargo.toml Cargo.lock ./
COPY src ./src

# Temporarily modify Cargo.toml to use edition 2021 for Docker build compatibility
RUN sed -i 's/edition = "2024"/edition = "2021"/' Cargo.toml

# Build the release binary
# Note: If you encounter SSL certificate issues during build, 
# you can use the simple build approach instead (see README)
RUN cargo build --release

# Runtime stage
FROM debian:bookworm-slim AS runtime

# Install runtime dependencies
RUN apt-get update && apt-get install -y \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Create a non-root user
RUN groupadd -r appuser && useradd -r -g appuser appuser

# Copy the binary from the builder stage
COPY --from=builder /app/target/release/client /usr/local/bin/client

# Set proper ownership and permissions
RUN chown appuser:appuser /usr/local/bin/client && \
    chmod +x /usr/local/bin/client

# Switch to non-root user
USER appuser

# Set the entrypoint
ENTRYPOINT ["/usr/local/bin/client"]

# ===== Alternative simple build approach =====
# If you encounter network issues during Docker build, you can use this simpler approach:
# 1. Build locally first: cargo build --release
# 2. Use this Dockerfile instead:
#
# FROM debian:bookworm-slim AS runtime
# RUN apt-get update && apt-get install -y ca-certificates && rm -rf /var/lib/apt/lists/*
# RUN groupadd -r appuser && useradd -r -g appuser appuser
# COPY target/release/client /usr/local/bin/client
# RUN chown appuser:appuser /usr/local/bin/client && chmod +x /usr/local/bin/client
# USER appuser
# ENTRYPOINT ["/usr/local/bin/client"]