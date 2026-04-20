# Printer

**Module:** `sixteeny.utils.printer`

A lightweight logging utility that writes messages to both the console and a log file.

---

## Class: `Printer`

```python
Printer(file_name: str, print_to_console: bool = True)
```

| Parameter | Type | Description |
|---|---|---|
| `file_name` | `str` | Path to the output log file |
| `print_to_console` | `bool` | Whether to echo messages to stdout (default: `True`) |

### Methods

```python
def print(
    self,
    text: str,
    end: str = "\n",
    dont_print_to_console: bool = False
) -> None
```
Log a message. Appends to the in-memory log; optionally prints to console.

| Parameter | Type | Description |
|---|---|---|
| `text` | `str` | Message to log (converted to string) |
| `end` | `str` | Line ending character (default: `"\n"`) |
| `dont_print_to_console` | `bool` | Suppress console output for this message only |

```python
def close(self) -> None
```
Flush the in-memory log to the file specified at construction. Call at experiment end to persist all logs.

---

## Usage Example

```python
from sixteeny.utils.printer import Printer

printer = Printer("experiment_log.txt", print_to_console=True)

printer.print("Experiment started")
printer.print(f"Arena 0: trial 1 started", end="\n")

# At end of experiment:
printer.close()
```

---

## Notes

- Messages are buffered in memory (`printer.log`) and only written to disk when `close()` is called. This avoids frequent disk I/O during the high-speed experiment loop.
- Progress checkpoints are printed every 100,000 lines during the file write.
