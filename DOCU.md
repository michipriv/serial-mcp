# Serial-MCP Complete Documentation

## 📖 About Serial-MCP

MCP server allowing Agents to talk to devices connected to serial port of the computer. Tested with MAC and Windows.

A robust serial communication server built with FastMCP framework, providing a reliable interface for serial port communication with features like message buffering, error handling, and connection management.

---

## Installation

1. Clone the repository:
```bash
git clone https://github.com/michipriv/serial-mcp.git
cd serial-mcp
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Configure Claude Desktop - see [CLAUDE_CONFIG.md](CLAUDE_CONFIG.md) for detailed setup instructions

---

## Features

- **Asynchronous Serial Communication**: Built on asyncio for efficient I/O operations
- **Message Buffering**: Configurable buffer size with timestamp support
- **Connection Management**: Automatic port detection and connection handling
- **Error Handling**: Comprehensive error detection and reporting
- **Runtime Configuration**: Configure ports and baud rates dynamically without config file changes
- **Logging**: Detailed logging with timestamps and error tracking
- **Enhanced Debugging**: Extended debug output for troubleshooting (this fork)

---

## Available Tools

### 1. delay
Wait for a specified number of seconds.

**Parameters:**
- `delay` (float): Number of seconds to wait (must be positive)

**Returns:**
- Success status
- Requested delay time
- Actual delay time (measured)
- Status message

**Example:**
```python
await delay({
    "delay": 2.5  # Wait for 2.5 seconds
})
```

---

### 2. init_serial
Initialize a serial connection with specified parameters.

**Parameters:**
- `port` (str): Serial port device path (e.g., '/dev/tty.usbmodem1101' or 'COM3')
- `baudrate` (int): Communication speed in bauds (default: 9600)
- `buffer_length` (int): Maximum number of messages to buffer (default: 100)

**Returns:**
- Status of initialization
- Current mode (real/disconnected)
- Port and baudrate information

---

### 3. send_message
Send a message through the serial connection and optionally wait for a response.

**Parameters:**
- `message` (str): Message to send
- `wait_for_response` (bool): Whether to wait for a response after sending (default: False)
- `response_timeout` (float): Time in seconds to wait for a response (default: 0.5)

**Returns:**
- `success` (bool): Overall operation success
- `status` (str): Operation status ("success", "error", etc.)
- `mode` (str): Current connection mode ("real", "disconnected")
- `write_status` (str): Write operation status:
  - "success": Message was written successfully
  - "failed": Write operation failed
  - "timeout": Write operation timed out
- `connection_state` (str): Current connection state:
  - "connected": Serial port is connected and available
  - "disconnected": No active serial connection
  - "error": Connection in error state
- `response_status` (str): Response status:
  - "not_checked": Response check was not requested
  - "received": Response was received
  - "no_response": No response received within timeout
  - "error": Error occurred while checking for response
- `response_messages` (list): List of received messages (if any)
- `bytes_written` (int): Number of bytes successfully written
- `error_type` (str): Type of error if operation failed
- `message` (str): Human-readable status message
- `error_details` (dict): Additional error information if applicable

**Example Send Without Response Check:**
```python
await send_message({
    "message": "Hello",
    "wait_for_response": False
})
# Returns:
{
    "success": True,
    "status": "success",
    "mode": "real",
    "write_status": "success",
    "connection_state": "connected",
    "response_status": "not_checked",
    "response_messages": [],
    "bytes_written": 6,
    "message": "Message sent successfully in real mode"
}
```

**Example Send With Response Check:**
```python
await send_message({
    "message": "Hello",
    "wait_for_response": True,
    "response_timeout": 1.0
})
# Returns:
{
    "success": True,
    "status": "success",
    "mode": "real",
    "write_status": "success",
    "connection_state": "connected",
    "response_status": "received",
    "response_messages": [
        {
            "timestamp": "2024-03-14T15:30:45.123456",
            "message": "Hello received"
        }
    ],
    "bytes_written": 6,
    "message": "Message sent successfully in real mode"
}
```

**Example No Response Within Timeout:**
```python
await send_message({
    "message": "Hello",
    "wait_for_response": True,
    "response_timeout": 0.5
})
# Returns:
{
    "success": True,
    "status": "success",
    "mode": "real",
    "write_status": "success",
    "connection_state": "connected",
    "response_status": "no_response",
    "response_messages": [],
    "bytes_written": 6,
    "message": "Message sent successfully in real mode"
}
```

---

### 4. read_message
Read messages from the buffer.

**Parameters:**
- `wait` (bool): Whether to wait for messages if buffer is empty (default: False)
- `timeout` (float): Time to wait for messages in seconds (default: 1.0)

**Returns:**
- List of messages with timestamps
- Current mode
- Status of read operation

---

### 5. list_serial_ports
List all available serial ports on the system.

**Parameters:**
None

**Returns:**
- List of available ports with details
- Port names and descriptions
- Hardware IDs

---

### 6. get_serial_status
Get the current status of the serial connection.

**Parameters:**
None

**Returns:**
- Current mode
- Port and baudrate information
- Buffer length
- Available ports
- Last error message
- Serial module availability

---

### 7. configure_serial
Configure the serial connection after initialization.

**Parameters:**
- `port` (str, optional): New port to use
- `baudrate` (int, optional): New baudrate to use
- `list_ports` (bool): Whether to just list ports (default: False)

**Returns:**
- Updated configuration
- Current mode
- Available ports (if list_ports is True)

---

### 8. close_serial
Close the current serial connection.

**Parameters:**
None

**Returns:**
- Status of closure
- Current mode

---

### 9. help
Get detailed instructions on how to use the Serial MCP server.

**Parameters:**
None

**Returns:**
- Detailed help information including:
  - Tool descriptions and parameters
  - Example usage for each tool
  - Common response formats
  - Connection states
  - Response statuses
  - Important usage notes

**Example:**
```python
await help()
# Returns comprehensive help information about all available tools and their usage
```

---

## Configuration

The server can be configured at runtime (recommended) or using environment variables:

- `SERIAL_PORT`: Serial port device path (default: '/dev/tty.usb*' on Mac, 'COM*' on Windows)
- `SERIAL_BAUD_RATE`: Communication speed in bauds (default: 9600)
- `SERIAL_BUFFER_LENGTH`: Maximum number of messages to buffer (default: 100)

**Note:** Runtime configuration through Claude is recommended. See [CLAUDE_CONFIG.md](CLAUDE_CONFIG.md) for setup.

---

## Usage

### Starting the Server

The server starts automatically when configured in Claude Desktop. See [CLAUDE_CONFIG.md](CLAUDE_CONFIG.md) for configuration.

### Runtime Configuration (Recommended)

```
You: "List available serial ports"
Claude: [Uses serial_MCP:list_serial_ports]

You: "Connect to COM5 at 115200 baud"
Claude: [Uses serial_MCP:init_serial]

You: "Send 'hello' and wait for response"
Claude: [Uses serial_MCP:send_message]
```

### API Endpoints (Programmatic Use)

#### Initialize Serial Connection
```python
await init_serial({
    "port": "/dev/tty.usbmodem1101",  # Mac/Linux
    # or "COM3",  # Windows
    "baudrate": 9600,
    "buffer_length": 100
})
```

#### Send Message
```python
await send_message({
    "message": "Hello, device!"
})
```

#### Read Messages
```python
await read_message({
    "wait": False,  # Whether to wait for messages
    "timeout": 1.0  # Timeout in seconds
})
```

#### List Available Ports
```python
await list_serial_ports()
```

#### Get Connection Status
```python
await get_serial_status()
```

#### Configure Connection
```python
await configure_serial({
    "port": "/dev/tty.usbmodem1101",
    "baudrate": 9600,
    "list_ports": False
})
```

#### Close Connection
```python
await close_serial()
```

---

## Message Format

Messages in the buffer are stored with timestamps:
```python
{
    "timestamp": "2024-03-14T15:30:45.123456",  # ISO format
    "message": "Actual message content"
}
```

---

## Error Handling

The server provides detailed error messages for various scenarios:
- Connection failures
- Invalid parameters
- Timeout conditions
- Buffer overflow
- Invalid messages

---

## Logging

Logs are written to `logs/serial_MCP.log` with the following format:
```
[timestamp] - [level] - [message]
```

**Enhanced in this fork:**
- DEBUG level logging enabled by default for detailed troubleshooting
- Additional port status logs during write operations
- Buffer status monitoring during response waits
- Detailed byte-waiting information

---

## Requirements

- Python 3.7+
- pyserial>=3.5
- fastmcp>=0.1.0
- pydantic>=2.0.0
- asyncio>=3.4.3

---

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## Acknowledgments

- [PaDev1/Serial-MCP](https://github.com/PaDev1/Serial-MCP) - Original project
- FastMCP framework for the server infrastructure
- pyserial for the serial communication capabilities