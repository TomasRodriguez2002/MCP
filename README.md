# MCP Server with Docker

This project demonstrates how to integrate the Model Control Protocol (MCP) with OpenAI's API, enabling OpenAI to access and use tools exposed by an MCP server running in Docker.

## Prerequisites

- Docker installed on your system
- Git (to clone the repository)

## Project Structure

- `server.py`: The MCP server implementation with a tool
- `client.py`: A client that connects to the server and calls the agent
- `Dockerfile`: Instructions for building the Docker image
- `requirements.txt`: Python dependencies for the project

### Data Flow Explanation

1. **User Query**: The user sends a query to the system (e.g., "What is our company's vacation policy?")
2. **OpenAI API**: OpenAI receives the query and available tools from the MCP server
3. **Tool Selection**: OpenAI decides which tools to use based on the query
4. **MCP Client**: The client receives OpenAI's tool call request and forwards it to the MCP server
5. **MCP Server**: The server executes the requested tool (e.g., retrieving knowledge base data)
6. **Response Flow**: The tool result flows back through the MCP client to OpenAI
7. **Final Response**: OpenAI generates a final response incorporating the tool data

## Running with Docker

### Step 1: Build the Docker image

```bash
docker build -t mcp-server .
```

### Step 2: Run the Docker container

```bash
docker run -p 8050:8050 mcp-server
```

This will start the MCP server inside a Docker container and expose it on port 8050.

## Running the Client

Once the server is running, you can run the client in a separate terminal:

```bash
python client.py
```

The client will connect to the server, list available tools, and call the agent to answer the query.

## Troubleshooting

If you encounter connection issues:

1. **Check if the server is running**: Make sure the Docker container is running with `docker ps`.

2. **Verify port mapping**: Ensure the port is correctly mapped with `docker ps` or by checking the output of the `docker run` command.

3. **Check server logs**: View the server logs with `docker logs <container_id>` to see if there are any errors.

4. **Host binding**: The server is configured to bind to `0.0.0.0` instead of `127.0.0.1` to make it accessible from outside the container. If you're still having issues, you might need to check your firewall settings.

5. **Network issues**: If you're running Docker on a remote machine, make sure the port is accessible from your client machine.

## Notes

- The server is configured to use SSE (Server-Sent Events) transport and listens on port 8050.
- The client connects to the server at `http://localhost:8050/sse`.
- Make sure the server is running before starting the client. 