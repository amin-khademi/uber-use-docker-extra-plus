# location_service/Dockerfile

# ---- Stage 1: Build ----
# Use the official Golang image to build the application.
FROM golang:1.22-alpine AS builder

WORKDIR /app

# 1. Copy go.mod and complete go.sum first for layer caching
COPY go.mod go.sum ./

# 2. Download dependencies based on verified checksums
RUN go mod download

# 3. Copy the source code into the container
COPY . .

# 4. Build the application statically
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o /go/bin/location-service .


# ---- Stage 2: Final ----
# Use a minimal 'scratch' image which contains nothing, just our app.
FROM scratch

COPY --from=builder /go/bin/location-service /location-service

# Expose both the application and the metrics ports
EXPOSE 8081
EXPOSE 9091

ENTRYPOINT ["/location-service"]