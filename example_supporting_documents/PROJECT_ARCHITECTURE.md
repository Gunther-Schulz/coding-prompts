# Project Architecture: LogTailer CLI

**Version:** 1.0
**Last Updated:** YYYY-MM-DD (Replace with actual date)

## 1. Introduction

This document outlines the architecture for `LogTailer`, a command-line interface (CLI) tool designed for real-time monitoring of log files. It allows users to tail multiple files, apply filters, highlight keywords, and customize output.

## 2. High-Level Architecture

LogTailer will employ a modular architecture focused on clear separation of concerns, making it maintainable and extensible.

*   **CLI Layer (Presentation):** Handles user interaction, command parsing, and output display.
*   **Core Logic Layer:** Contains the main functionality for file watching, log processing (parsing, filtering, highlighting), and configuration management.
*   **File System Interaction Layer:** Abstracted components for monitoring file changes.

```mermaid
graph TD
    User -- Interacts via --> CLI[CLI Interface (Typer/Click)]
    CLI -- Uses --> ConfigManager[Configuration Manager]
    CLI -- Controls --> CoreEngine[Core Engine]

    subgraph CoreLogic
        CoreEngine
        ConfigManager
        LogProcessor[Log Processor]
        FileWatcher[File Watcher Abstraction]
    end

    CoreEngine -- Uses --> LogProcessor
    CoreEngine -- Uses --> FileWatcher

    LogProcessor -- Composes --> Parser[Log Parser]
    LogProcessor -- Composes --> Filter[Log Filter]
    LogProcessor -- Composes --> Highlighter[Keyword Highlighter]

    FileWatcher -- Implemented by --> FSAdapter[File System Adapter (e.g., Watchdog)]

    FSAdapter -- Monitors --> LogFiles[Log Files on Disk]
    ConfigManager -- Reads/Writes --> ConfigFile[Configuration File (YAML/JSON)]
```

## 3. Key Components and Responsibilities

*   **`logtailer.cli` (or `main.py`):**
    *   Entry point of the application.
    *   Uses `Typer` (or `Click`) for command-line argument parsing (e.g., `start`, `add-file`, `config`).
    *   Orchestrates calls to the Core Logic Layer based on user commands.
    *   Manages terminal output, potentially using a library like `Rich` for enhanced display.
*   **`logtailer.core.engine`:**
    *   The central orchestrator.
    *   Manages a list of active log sources (files).
    *   Initializes and controls `FileWatcher` instances.
    *   Receives new log lines from watchers and passes them to the `LogProcessor`.
    *   Handles start/stop signals for tailing.
*   **`logtailer.core.watcher`:**
    *   Abstracts the file monitoring mechanism.
    *   Concrete implementations (e.g., `WatchdogWatcher`) detect changes in log files (new lines appended).
    *   Emits events (e.g., "new_line_detected") with the new content.
*   **`logtailer.core.processor`:**
    *   Receives raw log lines.
    *   **Parser:** Transforms raw log lines into structured data (if applicable, e.g., timestamp, level, message). Can be simple string splitting or regex-based.
    *   **Filter:** Applies user-defined filter rules (e.g., include/exclude based on keywords, log levels).
    *   **Highlighter:** Applies user-defined highlighting rules (e.g., color-code lines with "ERROR" in red).
    *   Passes processed and formatted log entries to the CLI layer for display.
*   **`logtailer.config.manager`:**
    *   Handles loading and saving of user configurations.
    *   Configuration includes:
        *   List of files/directories to watch.
        *   Filter rules (regex, keywords).
        *   Highlighting rules (keywords, colors).
        *   Default display preferences.
    *   Typically uses a YAML or JSON file (e.g., `~/.config/logtailer/config.yaml`).
    *   May use `Pydantic` for configuration validation.
*   **`logtailer.models` (Optional but Recommended):**
    *   Defines data structures (e.g., `LogEntry`, `FilterRule`, `HighlightRule`) using `Pydantic` for clarity and validation.

## 4. Directory Structure (Illustrative)

```
logtailer/
├── __init__.py
├── cli.py                  # CLI commands and main entry point
├── core/
│   ├── __init__.py
│   ├── engine.py           # Core orchestration logic
│   ├── watcher.py          # File watching abstraction and implementations
│   ├── processor.py        # Log parsing, filtering, highlighting
│   └── models.py           # Pydantic models for log entries, rules
├── config/
│   ├── __init__.py
│   └── manager.py          # Configuration loading and saving
├── utils/                  # Shared utility functions (e.g., color handling)
│   └── __init__.py
└── tests/                  # Unit and integration tests
    ├── core/
    └── config/
```

## 5. Key Libraries & Frameworks

*   **CLI Framework:** `Typer` or `Click` (for robust command-line interfaces).
*   **File Watching:** `watchdog` (for cross-platform file system event monitoring).
*   **Configuration Parsing:** `PyYAML` (for YAML) or standard `json` module. `Pydantic` for validation.
*   **Rich Display (Optional):** `Rich` (for enhanced terminal output, colors, tables).
*   **Testing:** `pytest`.

## 6. Workflow Example: `logtailer start --config myapp.yaml`

1.  **User Input:** User executes the command.
2.  **`cli.py`:**
    *   `Typer` parses the command and arguments (`start`, `config_file="myapp.yaml"`).
    *   The `start` command function is invoked.
3.  **`config.manager.load_config("myapp.yaml")`:**
    *   Loads configuration (files to watch, filters, highlights) from `myapp.yaml`.
    *   Validates the configuration.
4.  **`core.engine.initialize(config)`:**
    *   The engine receives the loaded configuration.
    *   For each configured file, it instantiates a `core.watcher` (e.g., `WatchdogWatcher`).
    *   It sets up the `core.processor` with filter and highlight rules from the config.
5.  **`core.engine.run()`:**
    *   Each `watcher` starts monitoring its assigned file.
    *   When a `watcher` detects new lines:
        *   It reads the new lines.
        *   It emits an event/callback to the `engine` with the new lines.
    *   The `engine` passes new lines to the `core.processor`.
    *   The `processor` parses, filters, and highlights the lines.
    *   The `processor` (or `engine` via the `processor`) sends the formatted lines to `cli.py` for display.
6.  **`cli.py` (Display):**
    *   Receives formatted log lines.
    *   Prints them to the console, applying colors/styles as determined by the `Highlighter` and potentially `Rich`.
7.  **Termination:** User presses `Ctrl+C`. The `engine` catches the signal, stops all watchers, and cleans up.

## 7. Configuration

Configuration will primarily be managed via a YAML or JSON file. Example structure (`config.yaml`):

```yaml
files_to_watch:
  - path: /var/log/app1.log
    tags: [app1, production]
  - path: /var/log/app2.log
    tags: [app2, staging]
    parser_type: json # Optional: specify if a special parser is needed

filters: # Processed in order
  - name: "Exclude DEBUG"
    type: exclude
    field: message # or level, if parsed
    pattern: "DEBUG"
    is_regex: false
  - name: "Include specific transaction"
    type: include
    field: message
    pattern: "transaction_id=xyz"
    is_regex: false

highlights:
  - name: "Errors in red"
    keyword: "ERROR"
    color: "red"
    case_sensitive: false
  - name: "Warnings in yellow"
    keyword: "WARN"
    color: "yellow"
    case_sensitive: false

display_preferences:
  timestamp_format: "%Y-%m-%d %H:%M:%S"
  show_tags: true
```

## 8. Error Handling

*   **File Not Found:** Gracefully handle and notify the user if configured log files are not accessible.
*   **Permission Errors:** Notify user of insufficient permissions to read files.
*   **Configuration Errors:** Provide clear messages if the configuration file is malformed or contains invalid settings (via Pydantic validation).
*   **Runtime Errors:** Log internal errors to a dedicated LogTailer log file or stderr for debugging.

## 9. Future Considerations

*   Plugin system for custom parsers or output formatters.
*   Remote log source support (e.g., via SSH or network streams).
*   Web UI for viewing logs (though the primary focus is CLI).
*   Integration with alerting systems.
