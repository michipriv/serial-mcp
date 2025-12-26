# Claude Desktop MCP Configuration for Serial-MCP

## Configuration File Location

### Windows
```
%APPDATA%\Claude\claude_desktop_config.json
```
Typical path: `C:\Users\<YourUsername>\AppData\Roaming\Claude\claude_desktop_config.json`

### macOS
```
~/Library/Application Support/Claude/claude_desktop_config.json
```

### Linux
```
~/.config/Claude/claude_desktop_config.json
```

---

## Configuration Example

Add this configuration to your `claude_desktop_config.json`:

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
        "C:\\Users\\YourUsername\\Serial-MCP\\serial_MCP.py"
      ]
    }
  }
}
```

**That's it!** No environment variables needed in the config. Port and baud rate are configured at runtime using Claude.

---

## Configuration Parameters

### Path Configuration
- **Replace** `C:\\Users\\YourUsername\\Serial-MCP\\serial_MCP.py` with your actual path
- **Windows:** Use double backslashes `\\` in paths
- **macOS/Linux:** Use forward slashes `/` (e.g., `/home/user/serial-mcp/serial_MCP.py`)

---

## Platform-Specific Examples

### Windows
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
        "C:\\Users\\mmade\\Serial-MCP\\serial_MCP.py"
      ]
    }
  }
}
```

### macOS
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
        "/Users/username/serial-mcp/serial_MCP.py"
      ]
    }
  }
}
```

### Linux
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
        "/home/user/serial-mcp/serial_MCP.py"
      ]
    }
  }
}
```

---

## Installation Steps

### 1. Install UV (if not already installed)
```bash
# Windows (PowerShell)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 2. Clone the Repository
```bash
git clone https://github.com/michipriv/serial-mcp.git
cd serial-mcp
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure Claude Desktop
1. Open `claude_desktop_config.json` in your editor
2. Add the serial_MCP configuration (see examples above)
3. Update the path to your serial_MCP.py file
4. Save the file

### 5. Restart Claude Desktop
Close and reopen Claude Desktop for the configuration to take effect.

---

## Usage - Runtime Configuration

**The Serial-MCP server is configured dynamically at runtime, not in the config file.**

### Step 1: Find Available Ports
In Claude chat:
```
List available serial ports
```

Claude will use `serial_MCP:list_serial_ports` to show connected devices.

### Step 2: Initialize Connection
In Claude chat:
```
Connect to COM5 at 115200 baud
```

or be more specific:
```
Initialize serial connection:
- Port: COM5
- Baud rate: 115200
- Buffer length: 100
```

Claude will call:
```json
serial_MCP:init_serial
{
  "port": "COM5",
  "baudrate": 115200,
  "buffer_length": 100
}
```

### Step 3: Send Commands
Once connected:
```
Send "uname -a" to the serial device and wait for response
```

Claude will handle the serial communication for you!

---

## Common Usage Examples

### Example 1: Rock Pi on Windows
```
1. "List available serial ports"
   → Shows: COM5 (USB-SERIAL CH340)

2. "Connect to COM5 at 115200 baud"
   → Connection established

3. "Send 'date' command and wait for response"
   → Sends command to Rock Pi
```

### Example 2: Arduino Development
```
1. "What serial ports are available?"
   → Shows: COM3 (Arduino Uno)

2. "Initialize serial on COM3 at 9600 baud"
   → Connected to Arduino

3. "Send 'LED ON' and wait 2 seconds for response"
   → Controls Arduino
```

### Example 3: ESP32 Communication
```
1. "Connect to /dev/ttyUSB0 at 115200"
   → Linux ESP32 connection

2. "Send AT commands to check WiFi status"
   → Communicates with ESP32
```

---

## Available Serial MCP Tools

Once configured, Claude can use these tools:

### 1. list_serial_ports
Find all available serial ports
```
"Show me available serial ports"
```

### 2. init_serial
Initialize connection with specific parameters
```
"Connect to COM5 at 115200 baud with buffer size 100"
```

### 3. send_message
Send data to the serial device
```
"Send 'hello' to the serial port"
"Send command and wait 2 seconds for response"
```

### 4. read_message
Read from the receive buffer
```
"Read any messages from the serial buffer"
```

### 5. get_serial_status
Check connection status
```
"What's the status of the serial connection?"
```

### 6. configure_serial
Change settings without reconnecting
```
"Change baud rate to 57600"
```

### 7. close_serial
Close the connection
```
"Close the serial connection"
```

### 8. delay
Wait for a specified time
```
"Wait 3 seconds before reading response"
```

---

## Advantages of Runtime Configuration

### Why No Config File Settings?

✅ **Flexibility** - Change ports/baud rates without editing config files  
✅ **Multiple Devices** - Switch between devices dynamically  
✅ **No Restart** - Reconfigure without restarting Claude  
✅ **Easier Debugging** - All settings visible in conversation  
✅ **Port Discovery** - Find ports at runtime, not in advance  

---

## Verification

After restarting Claude Desktop, verify the MCP server is working:

**In Claude chat:**
```
Can you list the available serial ports?
```

Expected response:
```
I can see the following serial ports:
- COM5: USB-SERIAL CH340 (COM5)
- COM6: Bluetooth Serial
...
```

---

## Troubleshooting

### MCP Server Not Showing Up
- ✅ Check the config file path is correct
- ✅ Verify JSON syntax (use a JSON validator)
- ✅ Ensure UV is installed (`uv --version`)
- ✅ Check file paths use correct separators (Windows: `\\`, Unix: `/`)
- ✅ Restart Claude Desktop completely

### Port Access Denied
- **Windows:** Run Claude Desktop as Administrator (if needed)
- **Linux:** Add user to dialout group: `sudo usermod -a -G dialout $USER` (logout required)
- **macOS:** Check System Preferences → Security & Privacy

### Dependencies Missing
```bash
pip install --break-system-packages fastmcp pyserial pydantic
```

### Can't Find Device
In Claude:
```
List all available serial ports with details
```
This shows hardware IDs and descriptions to identify your device.

### Connection Failed
Common issues:
- Wrong baud rate (check device documentation)
- Port already in use (close other serial programs)
- Wrong port name (use `list_serial_ports` to verify)

### Debug Logs
Check detailed logs at:
```
C:\Users\<Username>\Serial-MCP\logs\serial_MCP.log
```

This fork has enhanced DEBUG logging showing:
- Port status before/after write operations
- Buffer status during operations  
- Byte counts and waiting data
- Connection state changes

---

## Advanced Configuration

### Custom Python Environment
If you want to use a specific Python installation:

```json
{
  "mcpServers": {
    "serial_MCP": {
      "command": "C:\\Python311\\python.exe",
      "args": [
        "C:\\Users\\YourUsername\\Serial-MCP\\serial_MCP.py"
      ]
    }
  }
}
```

### Running Without UV
If you prefer not to use UV:

```json
{
  "mcpServers": {
    "serial_MCP": {
      "command": "python",
      "args": [
        "C:\\Users\\YourUsername\\Serial-MCP\\serial_MCP.py"
      ]
    }
  }
}
```

---

## Multiple Serial Devices

You can configure multiple instances of Serial-MCP for different devices:

```json
{
  "mcpServers": {
    "serial_MCP_device1": {
      "command": "uv",
      "args": [
        "run",
        "--with",
        "fastmcp,pyserial",
        "fastmcp",
        "run",
        "C:\\Path\\To\\serial_MCP.py"
      ]
    },
    "serial_MCP_device2": {
      "command": "uv",
      "args": [
        "run",
        "--with",
        "fastmcp,pyserial",
        "fastmcp",
        "run",
        "C:\\Path\\To\\serial_MCP.py"
      ]
    }
  }
}
```

Then in Claude:
```
"Use serial_MCP_device1 to connect to COM3"
"Use serial_MCP_device2 to connect to COM5"
```

---

## Additional Resources

- **Claude Desktop Documentation:** https://docs.claude.com/
- **MCP Documentation:** https://modelcontextprotocol.io/
- **Serial-MCP Repository:** https://github.com/michipriv/serial-mcp
- **Test Report:** See `TESTS.md` for stability test results

---

## Quick Start Summary

1. **Install UV:** `powershell -c "irm https://astral.sh/uv/install.ps1 | iex"`
2. **Clone repo:** `git clone https://github.com/michipriv/serial-mcp.git`
3. **Install deps:** `pip install -r requirements.txt`
4. **Add to Claude config:** Edit `claude_desktop_config.json` (just path, no env vars!)
5. **Restart Claude Desktop**
6. **In Claude:** "List available serial ports"
7. **Connect:** "Connect to COM5 at 115200 baud"
8. **Use it:** "Send 'hello' to serial port"

**That's it!** 🎉