# TechGeo Utility

A small Linux-based utility menu for system checks, file actions, port scanning, network discovery, and session logging.

## Usage

1. Compile the program:

   ```bash
   cd "/home/geoffrey/Desktop/auto cpp file"
   g++ -Wall -Wextra -Wpedantic -std=c++17 main.cpp -o output/main
   ```

2. Run the program:

   ```bash
   cd "/home/geoffrey/Desktop/auto cpp file/output"
   ./main
   ```

3. When prompted, enter a file name for saving the session output.

4. Select an option from the menu.

5. Use the final menu option to exit the program.

## Important warnings

### 1. File names must be unique
- If the file already exists, the program will refuse it.
- The user must choose a different name or delete the existing file first.
- This prevents accidental overwrite of old session logs.

### 2. Root and user switching
- The app may ask whether to re-run as root.
- The Switch User option is available near the exit section.
- If the app is running as root, it may switch to a normal user.
- If it is running as a normal user, it may try to re-run as root using sudo.
- Use this carefully because it changes the active session permissions.

### 3. Network scanning can be slow
- Port scanning and device discovery may take time depending on the network size.
- Some scans can be resource-intensive.
- Use them only on networks you are authorized to test.

### 4. Security scan is for authorized testing only
- The Security Scan feature connects to a target IP and port.
- This can interact with services on a machine.
- Do not run it against systems you do not own or have permission to test.

### 5. Command execution is powerful
- The Run Command option executes shell commands.
- It saves the command output into the selected output file only when the command succeeds.
- Be careful with commands that modify system state or delete data.

### 6. Output file is the main session log
- Results from many functions are written to the selected file.
- This file is used as the program's execution report and saved output.

### 7. Dump log and background process output
- The selected file starts with the `=== dumping files ===` heading, followed by machine and session metadata.
- When the background system starts, its startup status is appended to that same file.
- The decryptor's standard output and error output are appended to the file while it runs. This console output appears after the startup information; it is not a copy of separate files the decryptor may create.
- Files created by the decryptor remain at the paths chosen by the decryptor. The program does not currently gather their contents into the selected log or add a closing dump marker.
- Keep the output file and any files created by the decryptor in a location you control, and use the background server only in an authorized, isolated environment.

## Notes

- This utility is intended for controlled, authorized environment use.
- Always confirm you have permission before scanning networks or connecting to systems.
- The output file is created and prepended with machine/session metadata when the program starts.
