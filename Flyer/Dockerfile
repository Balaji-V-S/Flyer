# -------- Build Stage --------
FROM ghcr.io/graalvm/native-image-community:21 AS builder

WORKDIR /app

# Install build tools
RUN microdnf install -y maven && microdnf clean all

# Copy Maven files first for dependency caching
COPY pom.xml .
RUN mvn -B -q dependency:go-offline

# Copy source
COPY src ./src

# Build native image
RUN mvn -Pnative -DskipTests package

# -------- Runtime Stage --------
FROM gcr.io/distroless/base-debian12

WORKDIR /app

# Copy the native binary
COPY --from=builder /app/target/*-runner /app/flyer-core

# Expose API port
EXPOSE 8080

# Use non-root user (distroless default)
USER nonroot:nonroot

# Run Flyer Core
ENTRYPOINT ["/app/flyer-core"]
