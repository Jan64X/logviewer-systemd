# Logviewer-systemd

Logviewer-systemd is a command-line tool that filters and saves system logs based on specified criteria.

This fork is a complete re-do, the original project used rust for basically just aliases which is completely unnecesary and also BLOAT.
This fork aims to remedy that.

## If it does not show anything, run it as root

## Usage


| Command | Description |
| --- | --- |
| `logviewer --help` | Display the help message. |
| `logviewer -s <path>` | Save logs to the specified path. |
| `logviewer -k <keyword>` | Filter logs by the specified keyword. |
| `logviewer -u <unit>` | Filter logs by the specified unit. |
| `logviewer -r` | Logging in real time |


## Dependencies

[Bash](https://www.gnu.org/software/bash/)
or
[Zsh](https://zsh.sourceforge.io/)

# Installation 
## Add this to your .bashrc or .zshrc
```bash
logviewer() {
    local save_path=""
    local keyword=""
    local unit=""
    local realtime=false
    local cmd="journalctl"

    # Parse command-line arguments
    while [[ $# -gt 0 ]]; do
        case "$1" in
            --help)
                $cmd --help
                return 0
                ;;
            -s)
                shift
                save_path="$1"
                ;;
            -k)
                shift
                keyword="$1"
                ;;
            -u)
                shift
                unit="$1"
                ;;
            -r)
                realtime=true
                ;;
            *)
                echo "Unknown option: $1"
                echo "Usage: logviewer [--help] [-s <path>] [-k <keyword>] [-u <unit>] [-r]"
                return 1
                ;;
        esac
        shift
    done

    # Build the journalctl command
    if [[ -n "$unit" ]]; then
        cmd="$cmd -u $unit"
    fi
    if [[ "$realtime" == true ]]; then
        cmd="$cmd -f"
    fi
    if [[ -n "$keyword" ]]; then
        cmd="$cmd | grep -i \"$keyword\""
    fi
    if [[ -n "$save_path" ]]; then
        eval "$cmd > \"$save_path\""
    else
        eval "$cmd"
    fi
}
```

