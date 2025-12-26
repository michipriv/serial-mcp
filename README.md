# Serial-MCP

## 🔧 Fork with Windows Optimizations and Debug Improvements

**This fork of [PaDev1/Serial-MCP](https://github.com/PaDev1/Serial-MCP) includes the following improvements:**

### ✨ Changes in This Fork

#### 🐛 Debug & Logging Enhancements
- **Enhanced Logging**: Logging level increased from `INFO` to `DEBUG` for more detailed diagnostics
- **Extended Port Status Logs**: New debug outputs show port status before/after write operations
- **Buffer Status Monitoring**: Additional logs when waiting for responses show buffer status

#### 🔍 Code Improvements
- **Improved Buffer Checking**: `if self.buffer is None` instead of `if not self.buffer` for correct null checking
- **Detailed Byte-Waiting Logs**: Shows exact number of waiting bytes (`Data available: X bytes waiting`)
- **Better Timeout Handling**: Extended debug outputs for response timeout operations

#### 📝 Useful For
- Debugging serial communication issues
- Development on Windows
- Diagnosis of timeout and buffer problems
- Detailed analysis of write/read operations

---

## 📖 About Serial-MCP

MCP server allowing Agents to talk to devices connected to serial port of the computer. Tested with MAC and Windows.

A robust serial communication server built with FastMCP framework, providing a reliable interface for serial port communication with features like message buffering, error handling, and connection management.

## Quick Start

### Installation

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

### Basic Configuration

Add to your Claude Desktop config (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "serial_MCP": {
      "command": "uv",
      "args": [
        "run",
        "--with",
        "fastmcp,pyserial",
        "fastmcp",
        "run",
        "<path to>/serial_MCP.py"
      ]
    }
  }
}
```

**No environment variables needed!** Port and baud rate are configured dynamically at runtime through Claude.

For complete setup instructions, see **[CLAUDE_CONFIG.md](CLAUDE_CONFIG.md)**

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

## Usage

### Runtime Configuration (Recommended)

Serial-MCP is designed to be configured at runtime through natural language conversation with Claude:

```
You: "List available serial ports"
Claude: [Uses serial_MCP:list_serial_ports]
        "Available ports: COM5 (USB-SERIAL CH340), COM3 (Arduino)..."

You: "Connect to COM5 at 115200 baud"
Claude: [Uses serial_MCP:init_serial with port=COM5, baudrate=115200]
        "Connected successfully to COM5 at 115200 baud"

You: "Send 'uname -a' and wait for response"
Claude: [Uses serial_MCP:send_message]
        "Sent command, received: Linux rockpi-e..."
```

### Available Tools

- **list_serial_ports** - Discover available serial ports
- **init_serial** - Initialize connection with port, baud rate, buffer settings
- **send_message** - Send data and optionally wait for response
- **read_message** - Read from receive buffer
- **get_serial_status** - Check connection status
- **configure_serial** - Change settings without reconnecting
- **close_serial** - Close connection
- **delay** - Wait for specified time
- **help** - Get detailed tool documentation

For detailed tool descriptions and examples, see the original documentation below.

---

## Documentation

- **[CLAUDE_CONFIG.md](CLAUDE_CONFIG.md)** - Complete Claude Desktop configuration guide
- **[TESTS.md](TESTS.md)** - Comprehensive stability test report
- **[README_ORIGINAL.md](README_ORIGINAL.md)** - Original project documentation with full API reference

---

## Test Results

This fork has been extensively tested and validated:

✅ **All Tests Passed** - See [TESTS.md](TESTS.md) for complete report
- 4+ minutes stability testing
- 20+ messages sent successfully  
- 100% success rate
- Perfect timeout handling
- No memory leaks detected

**Tested Configuration:**
- Device: Rock Pi (USB-Serial CH340)
- Port: COM5
- Baud Rate: 115200
- Platform: Windows

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

---

## Fork Maintainer

This fork is maintained by [@michipriv](https://github.com/michipriv) with focus on Windows compatibility and enhanced debugging capabilities.

---

## Links

- **Original Project:** [PaDev1/Serial-MCP](https://github.com/PaDev1/Serial-MCP)
- **This Fork:** [michipriv/serial-mcp](https://github.com/michipriv/serial-mcp)
- **Issues:** [Report an Issue](https://github.com/michipriv/serial-mcp/issues)