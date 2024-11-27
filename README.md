
# Simple Load Balancer in Go

This project implements a basic round-robin load balancer in Go using the `net/http` package and the `httputil.ReverseProxy`. It forwards incoming HTTP requests to a pool of backend servers and cycles through them in a round-robin fashion.

## Features

- **Round-Robin Load Balancing**: Distributes incoming requests across multiple backend servers in a round-robin manner.
- **Reverse Proxy**: Forwards requests from the load balancer to the actual backend servers using `httputil.ReverseProxy`.
- **Server Health Check (Simple)**: Each server is assumed to be alive (can be extended for real health checks).
  
## Components

- **`Server` Interface**: Defines the basic contract for any server in the pool, including methods to retrieve the server address and check if the server is alive.
- **`simpleServer`**: Implements the `Server` interface and uses a reverse proxy to forward requests to an actual server.
- **`LoadBalancer`**: Manages a list of backend servers and directs incoming requests to them in a round-robin fashion.

## How It Works

1. **Incoming Request**: When the load balancer receives a request, it selects a backend server using the round-robin algorithm.
2. **Forwarding**: The request is forwarded to the selected server using Go’s `httputil.ReverseProxy`.
3. **Response**: The backend server processes the request and sends the response back through the load balancer to the client.

## Prerequisites

## Setup and Run

1. Clone the repository:
    ```bash
    git clone https://github.com/yourusername/go-loadbalancer.git
    cd go-loadbalancer
    ```

2. Build and run the application:
    ```bash
    go run main.go
    ```

3. Open your browser and visit `http://localhost:3000/`. The request will be forwarded to one of the configured backend servers (Facebook, Twitter, or Google in this example).

## Configuration

### Adding/Removing Backend Servers

To change the backend servers, modify the list in the `main` function:

```go
servers := []Server{
    newSimpleServer("http://www.facebook.com"),
    newSimpleServer("http://www.twitter.com"),
    newSimpleServer("http://www.google.com"),
}
```

You can replace these URLs with the addresses of your own servers.

### Changing the Load Balancing Algorithm

Currently, the load balancer uses a simple round-robin algorithm. You can implement different strategies (e.g., least connections, weighted round-robin) by modifying the `getNextAvailableServer` method in the `LoadBalancer` struct.

## Project Structure

```
.
├── main.go         # Main Go file that contains the load balancer logic
├── README.md       # This README file
```

## Example

When you run the load balancer and make multiple requests to `http://localhost:3000/`, you should see logs like the following, showing which backend server is being used:

```
serving request at localhost: 3000
forwarding request to http://www.facebook.com
forwarding request to http://www.twitter.com
forwarding request to http://www.google.com
forwarding request to http://www.facebook.com
...
```

