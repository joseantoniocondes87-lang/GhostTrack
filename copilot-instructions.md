# GhostTrack - AI Coding Agent Instructions

## Project Overview
GhostTrack is a Python-based OSINT (Open Source Intelligence) tool for tracking IP addresses, phone numbers, and usernames across social media platforms. Single-file CLI application (`GhostTR.py`) with menu-driven interface.

## Architecture & Code Structure

### Core Design Pattern
- **Single-file monolithic**: All functionality in `GhostTR.py` (319 lines)
- **Decorator pattern**: `@is_option` decorator wraps menu functions to auto-display banner before execution
- **Menu-driven**: `options` list defines menu structure with `num`, `text`, and `func` mappings
- **Function dispatch**: `call_option()` dynamically invokes functions based on user selection

### Key Components
1. **IP Tracking** (`IP_Track()`): Uses ipwho.is API, displays 25+ data points including geolocation
2. **Phone Tracking** (`phoneGW()`): Leverages `phonenumbers` library for carrier/location lookup (defaults to Indonesia region "ID")
3. **Username Tracking** (`TrackLu()`): Checks 24 social media platforms via HTTP status codes
4. **IP Display** (`showIP()`): Shows user's public IP via ipify.org API

## Developer Workflows

### Running the Tool
```bash
python3 GhostTR.py
```
No build process. Direct execution only.

### Dependencies
```bash
pip3 install -r requirements.txt
```
Only two external libraries: `requests`, `phonenumbers`

### Testing Approach
No automated tests exist. Manual testing through menu navigation required.

## Project-Specific Conventions

### Color Coding System
Uses ANSI escape codes for terminal colors:
- `Wh` (white): Prompts and labels
- `Gr` (green): User data and results
- `Re` (red): Errors
- `Ye` (yellow): Warnings
Always use these variables for consistency, never raw escape codes.

### Error Handling Pattern
- `KeyboardInterrupt` caught at two levels: `main()` and `execute_option()`
- Try-except with generic `Exception` in API-calling functions
- No custom exceptions or detailed error logging

### Banner Display
- `run_banner()`: ASCII ghost art, shown before each tracking operation
- `option()`: Main menu banner with tool name
- Both use `stderr.writelines()` for output (unusual but intentional)

### Input/Output Pattern
```python
input(f"{Wh}\n Enter X : {Gr}")  # Prompt in white, input in green
print(f"{Wh} Label :{Gr} {value}")  # Results format
```

## API Integration Points

### External Services
1. **ipwho.is**: No auth required, returns JSON with 20+ fields
2. **ipify.org**: Text response with public IP
3. **Social media platforms**: Simple GET requests, 200 = exists

### Phone Number Handling
- Default region: `"ID"` (Indonesia) hardcoded in `phoneGW()`
- Input format: `+6281xxxxxxxxx` (international format expected)
- Uses `phonenumbers.parse()` with explicit region parameter

## Critical Implementation Details

### Menu System
```python
options = [
    {'num': 1, 'text': 'IP Tracker', 'func': IP_Track},
    # ...
    {'num': 0, 'text': 'Exit', 'func': exit}
]
```
- Option 0 always reserved for exit
- `execute_option()` loops back to `main()` after completion
- Invalid input triggers recursive retry with 2-second delay

### Platform Detection
```python
if os.name == 'nt':  # Windows
    os.system('cls')
else:  # Unix-like
    os.system('clear')
```
Use `os.name` check, not `platform` module.

## Licensing & Attribution
Headers include Islamic references and strict recoding policy:
- Must request permission before forking/modifying
- Must credit original author (HunxByts/Hunx04)
- Religious warnings about unauthorized use
**When modifying**: Preserve header comments unchanged.

## Common Pitfalls
- Don't add async/await - tool uses blocking `time.sleep()` deliberately for UX
- Don't refactor into multiple files without explicit request (intentionally single-file)
- Don't remove `stderr.writelines()` - intentional choice for banner output
- API keys not needed for any current integrations
