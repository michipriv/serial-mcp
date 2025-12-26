# Serial-MCP - Deutsche Dokumentation

## 🔧 Fork mit Windows-Optimierungen und Debug-Verbesserungen

**Dieser Fork von [PaDev1/Serial-MCP](https://github.com/PaDev1/Serial-MCP) enthält folgende Verbesserungen:**

### ✨ Änderungen in diesem Fork

#### 🐛 Debug & Logging Verbesserungen
- **Enhanced Logging**: Logging-Level von `INFO` auf `DEBUG` erhöht für detailliertere Diagnose
- **Erweiterte Port-Status-Logs**: Neue Debug-Ausgaben zeigen Port-Status vor/nach Schreiboperationen
- **Buffer-Status-Monitoring**: Zusätzliche Logs beim Warten auf Antworten zeigen Buffer-Status

#### 🔍 Code-Verbesserungen
- **Verbesserte Buffer-Prüfung**: `if self.buffer is None` statt `if not self.buffer` für korrekte Null-Prüfung
- **Detaillierte Byte-Waiting-Logs**: Zeigt exakte Anzahl wartender Bytes an (`Data available: X bytes waiting`)
- **Besseres Timeout-Handling**: Erweiterte Debug-Ausgaben für Response-Timeout-Vorgänge

#### 📝 Nützlich für
- Debugging von seriellen Kommunikationsproblemen
- Entwicklung unter Windows
- Diagnose von Timeout- und Buffer-Problemen
- Detaillierte Analyse von Schreib-/Lesevorgängen

---

## 📖 Über Serial-MCP

MCP-Server, der es Agenten ermöglicht, mit Geräten zu kommunizieren, die an die serielle Schnittstelle des Computers angeschlossen sind. Getestet mit MAC und Windows.

Ein robuster serieller Kommunikationsserver, der mit dem FastMCP-Framework erstellt wurde und eine zuverlässige Schnittstelle für die serielle Port-Kommunikation mit Funktionen wie Nachrichten-Pufferung, Fehlerbehandlung und Verbindungsverwaltung bietet.

---

## Schnellstart

### Installation

1. Repository klonen:
```bash
git clone https://github.com/michipriv/serial-mcp.git
cd serial-mcp
```

2. Abhängigkeiten installieren:
```bash
pip install -r requirements.txt
```

3. Claude Desktop konfigurieren - siehe [CLAUDE_CONFIG.md](CLAUDE_CONFIG.md) für detaillierte Anweisungen

### Basis-Konfiguration

Zur Claude Desktop Config hinzufügen (`claude_desktop_config.json`):

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
        "<Pfad zu>/serial_MCP.py"
      ]
    }
  }
}
```

**Keine Environment-Variablen nötig!** Port und Baudrate werden zur Laufzeit dynamisch über Claude konfiguriert.

Für vollständige Einrichtungsanleitung siehe **[CLAUDE_CONFIG.md](CLAUDE_CONFIG.md)**

---

## Funktionen

- **Asynchrone serielle Kommunikation**: Basiert auf asyncio für effiziente I/O-Operationen
- **Nachrichten-Pufferung**: Konfigurierbare Puffergröße mit Zeitstempel-Unterstützung
- **Verbindungsverwaltung**: Automatische Port-Erkennung und Verbindungsbehandlung
- **Fehlerbehandlung**: Umfassende Fehlererkennung und -meldung
- **Laufzeit-Konfiguration**: Ports und Baudraten dynamisch ohne Config-Datei-Änderungen konfigurieren
- **Logging**: Detailliertes Logging mit Zeitstempeln und Fehler-Tracking
- **Erweitertes Debugging**: Erweiterte Debug-Ausgabe zur Fehlersuche (dieser Fork)

---

## Verwendung

### Laufzeit-Konfiguration (Empfohlen)

Serial-MCP ist darauf ausgelegt, zur Laufzeit durch natürlichsprachliche Konversation mit Claude konfiguriert zu werden:

```
Du: "Zeige verfügbare serielle Ports"
Claude: [Verwendet serial_MCP:list_serial_ports]
        "Verfügbare Ports: COM5 (USB-SERIAL CH340), COM3 (Arduino)..."

Du: "Verbinde mit COM5 mit 115200 Baud"
Claude: [Verwendet serial_MCP:init_serial mit port=COM5, baudrate=115200]
        "Erfolgreich mit COM5 bei 115200 Baud verbunden"

Du: "Sende 'uname -a' und warte auf Antwort"
Claude: [Verwendet serial_MCP:send_message]
        "Befehl gesendet, Antwort erhalten: Linux rockpi-e..."
```

### Verfügbare Tools

- **list_serial_ports** - Verfügbare serielle Ports finden
- **init_serial** - Verbindung mit Port, Baudrate, Puffer-Einstellungen initialisieren
- **send_message** - Daten senden und optional auf Antwort warten
- **read_message** - Aus Empfangspuffer lesen
- **get_serial_status** - Verbindungsstatus prüfen
- **configure_serial** - Einstellungen ändern ohne erneute Verbindung
- **close_serial** - Verbindung schließen
- **delay** - Auf bestimmte Zeit warten
- **help** - Detaillierte Tool-Dokumentation erhalten

---

## Dokumentation

- **[CLAUDE_CONFIG.md](CLAUDE_CONFIG.md)** - Vollständige Claude Desktop Konfigurationsanleitung
- **[TESTS.md](TESTS.md)** - Umfassender Stabilitäts-Testbericht
- **[README_ORIGINAL.md](README_ORIGINAL.md)** - Original-Projektdokumentation mit vollständiger API-Referenz

---

## Testergebnisse

Dieser Fork wurde ausführlich getestet und validiert:

✅ **Alle Tests bestanden** - Siehe [TESTS.md](TESTS.md) für vollständigen Bericht
- 4+ Minuten Stabilitätstests
- 20+ Nachrichten erfolgreich gesendet
- 100% Erfolgsrate
- Perfektes Timeout-Handling
- Keine Memory-Leaks erkannt

**Getestete Konfiguration:**
- Gerät: Rock Pi (USB-Serial CH340)
- Port: COM5
- Baudrate: 115200
- Plattform: Windows

---

## Anforderungen

- Python 3.7+
- pyserial>=3.5
- fastmcp>=0.1.0
- pydantic>=2.0.0
- asyncio>=3.4.3

---

## Mitwirken

1. Repository forken
2. Feature-Branch erstellen (`git checkout -b feature/tolles-feature`)
3. Änderungen committen (`git commit -m 'Tolles Feature hinzugefügt'`)
4. Branch pushen (`git push origin feature/tolles-feature`)
5. Pull Request öffnen

---

## Lizenz

Dieses Projekt ist unter der MIT-Lizenz lizenziert - siehe LICENSE-Datei für Details.

---

## Danksagungen

- [PaDev1/Serial-MCP](https://github.com/PaDev1/Serial-MCP) - Original-Projekt
- FastMCP-Framework für die Server-Infrastruktur
- pyserial für die seriellen Kommunikationsfähigkeiten

---

## Fork-Maintainer

Dieser Fork wird von [@michipriv](https://github.com/michipriv) gewartet mit Fokus auf Windows-Kompatibilität und erweiterte Debugging-Fähigkeiten.

---

## Links

- **Original-Projekt:** [PaDev1/Serial-MCP](https://github.com/PaDev1/Serial-MCP)
- **Dieser Fork:** [michipriv/serial-mcp](https://github.com/michipriv/serial-mcp)
- **Issues:** [Problem melden](https://github.com/michipriv/serial-mcp/issues)