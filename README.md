1. **Introduction**
2. **Environment Setup**
3. **Detailed Configuration**
4. **Testing**
5. **Troubleshooting**
6. **Additional Information**

```markdown
# Janus WebRTC Gateway Docker Setup

## Introduction

This documentation provides a step-by-step guide to setting up the Janus WebRTC Gateway using Docker. It covers pulling the Docker image, running the container, verifying configurations, and testing the setup.

## Environment Setup

Ensure the following prerequisites are met:
- **Docker**: Make sure Docker is installed on your system.
- **Basic Knowledge**: Familiarity with WebRTC and Docker concepts.

## Steps

### 1. **Pull the Docker Image**

Pull the Janus WebRTC Gateway image from Docker Hub:

```bash
docker pull canyan/janus-gateway:latest
```

### 2. **Run the Janus Container**

Start the Janus container with the following command:

```bash
docker run -d --name janus-gateway \
  -p 8088:8088 \
  -p 8188:8188 \
  canyan/janus-gateway:latest
```

**Parameters:**
- `-d`: Run the container in detached mode.
- `--name janus-gateway`: Name the container `janus-gateway`.
- `-p 8088:8088`: Map port 8088 on the host to port 8088 in the container.
- `-p 8188:8188`: Map port 8188 on the host to port 8188 in the container.

### 3. **Verify Container Status**

Check the status of the running containers:

```bash
docker ps
```

### 4. **Check Container Logs**

View the logs of the Janus container to ensure it started correctly:

```bash
docker logs janus-gateway
```

### 5. **Verify Network Ports**

Ensure the HTTP and WebSocket ports are listening:

```bash
sudo netstat -tuln | grep 8088
sudo netstat -tuln | grep 8188
```

### 6. **Test HTTP Endpoint**

Verify that the Janus HTTP endpoint is accessible:

```bash
curl http://localhost:8088/janus/info
```

Expected response indicates Janus is running.

### 7. **Test WebSocket Connection**

To test WebSocket functionality:

- **Using `websocat`:**
  ```bash
  websocat ws://localhost:8188/
  ```

- **Using Browser Developer Tools:**
  ```javascript
  const ws = new WebSocket('ws://localhost:8188/');
  ws.onopen = () => console.log('WebSocket connection opened');
  ws.onmessage = (event) => console.log('Message from server:', event.data);
  ws.onerror = (error) => console.error('WebSocket error:', error);
  ws.onclose = () => console.log('WebSocket connection closed');
  ```

- **Using `wscat`:**
  ```bash
  npm install -g wscat
  wscat -c ws://localhost:8188/
  ```

### 8. **Configuration Details**

Check the configuration file for Janus, typically located at `/usr/local/etc/janus/janus.cfg`. Ensure that WebSocket settings are correctly configured:

```ini
[general]
ws = true
ws_port = 8188
wss = false
```

### 9. **Troubleshooting**

- **WebSocket Connection Issues:**
  - Verify Janus configuration (`janus.cfg`).
  - Ensure WebSocket port is correctly mapped and open.
  - Check container logs for errors related to WebSocket initialization.

- **HTTP Endpoint Issues:**
  - Confirm Janus is running and the container is mapped to the host port.
  - Review container logs for HTTP transport errors.

### 10. **Additional Information**

- **Janus WebRTC Gateway Documentation:** [Janus Docs](https://janus.conf.meetecho.com/docs/)
- **Docker Documentation:** [Docker Docs](https://docs.docker.com/)

## Conclusion

You have successfully set up and tested the Janus WebRTC Gateway using Docker. Both HTTP and WebSocket endpoints are confirmed to be operational. For further integration with WebRTC applications, refer to the Janus documentation.

```

This structure provides a more comprehensive overview of your setup process and includes sections for troubleshooting and additional resources. Adjust or expand based on your specific needs.
