# Development Guide

This guide covers how to build, test, and run chrome-cli locally.

## Prerequisites

- **macOS** (required - uses native Apple frameworks)
- **Xcode** (install from the App Store or via `xcode-select --install` for command-line tools)

## Building

### Using Xcode GUI

1. Open the project:
   ```
   open chrome-cli.xcodeproj
   ```
2. Build with **Cmd+B** (or Product → Build)
3. The executable is placed in the build output directory (typically `DerivedData/`)

### Using Command Line

Build with xcodebuild:

```bash
xcodebuild -project chrome-cli.xcodeproj -scheme chrome-cli -configuration Release build
```

To build to a specific location:

```bash
xcodebuild -project chrome-cli.xcodeproj -scheme chrome-cli -configuration Release SYMROOT=./build
```

This places the binary at `./build/Release/chrome-cli`.

## Running Your Local Build

After building, run your local binary directly:

```bash
./build/Release/chrome-cli help
./build/Release/chrome-cli list tabs
./build/Release/chrome-cli version
```

Test with different browsers using the `CHROME_BUNDLE_IDENTIFIER` environment variable:

```bash
CHROME_BUNDLE_IDENTIFIER="com.brave.Browser" ./build/Release/chrome-cli list tabs
```

## Testing

There is no automated test suite. Testing is manual:

1. Build your changes
2. Run the binary against a running Chrome instance
3. Verify commands work as expected:
   ```bash
   ./build/Release/chrome-cli list tabs
   ./build/Release/chrome-cli list windows
   ./build/Release/chrome-cli info
   ```

**Note**: Some features require enabling "Allow JavaScript from Apple Events" in Chrome's View → Developer menu.

## Project Structure

| Path | Description |
|------|-------------|
| `chrome-cli/App.m` | Main logic - all commands implemented here |
| `chrome-cli/App.h` | App interface declarations |
| `chrome-cli/main.m` | Entry point and argument routing |
| `chrome-cli/chrome.h` | ScriptingBridge interface to Chrome |
| `chrome-cli/Argonaut.m` | Command-line argument parser |
| `chrome-cli/Handler.m` | Command handler dispatcher |
| `scripts/` | Wrapper scripts for other browsers (Brave, Edge, etc.) |

## Quick Development Workflow

```bash
# 1. Make your code changes in chrome-cli/*.m files

# 2. Build from command line
xcodebuild -project chrome-cli.xcodeproj -scheme chrome-cli -configuration Debug SYMROOT=./build

# 3. Test your changes
./build/Debug/chrome-cli <your-command>
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `CHROME_BUNDLE_IDENTIFIER` | Override target browser (e.g., `com.brave.Browser`) |
| `OUTPUT_FORMAT` | Set to `json` for JSON output (defaults to text) |
