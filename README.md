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

## 📚 Documentation

For complete documentation, installation instructions, API reference, and usage examples, see:

👉 **[DOCU.md](DOCU.md)** - Complete Documentation

👉 **[CLAUDE_CONFIG.md](CLAUDE_CONFIG.md)** - Claude Desktop Setup Guide

👉 **[TESTS.md](TESTS.md)** - Stability Test Report

---

## Quick Links

- **Original Project:** [PaDev1/Serial-MCP](https://github.com/PaDev1/Serial-MCP)
- **This Fork:** [michipriv/serial-mcp](https://github.com/michipriv/serial-mcp)
- **Issues:** [Report an Issue](https://github.com/michipriv/serial-mcp/issues)

---

**Fork maintained by [@michipriv](https://github.com/michipriv)**
