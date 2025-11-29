# chrome-cookie-stealer-py
A Python script designed to find, decrypt, and exfiltrate Google Chrome cookies from a Windows machine that I made for a YT video

## Description

This script performs the following actions:
1. Locates the Chrome `Local State` and `Cookies` database files in the current user's AppData directory.
2. Extracts the encrypted master key from the `Local State` file and decrypts it using the Windows Data Protection API (DPAPI) via the `win32crypt` module.
3. Creates a temporary copy of the `Cookies` SQLite database to bypass potential file locks.
4. Iterates through all cookies in the database, decrypting the `encrypted_value` for each entry using AES-GCM with the previously obtained master key.
5. Structures the decrypted cookie data into a JSON format.
6. Establishes a TCP socket connection to a predefined IP address and port.
7. Transmits the complete JSON-formatted cookie data to the connected server.

## Dependencies

- `pywin32` (for `win32crypt`)
- `pycryptodomex` (for `Cryptodome.Cipher.AES`)

You can install them using pip:
```bash
pip install pywin32 pycryptodomex
```

## Configuration

Before use, you must modify the following variables in the script:
- `IP`: The IP address of your Command & Control (C2) server.
- `PORT`: The port on which your C2 server is listening.

## Usage

This script is intended to be run on the target Windows machine where Google Chrome is installed and has been used.

### Compiling to an Executable

The recommended method of deployment is to compile the script into a standalone `.exe` file using PyInstaller. This creates a self-contained executable that does not require a console window to be open, making it discreet.

1.  **Install PyInstaller:**
    ```bash
    pip install pyinstaller
    ```

2.  **Compile the script:**
    Navigate to the directory containing your Python script and run the following command:
    ```bash
    pyinstaller --onefile --noconsole your_script_name.py
    ```
    - `--onefile`: Bundles all dependencies and the script into a single executable.
    - `--noconsole`: Prevents a command-line window from appearing when the executable is run.

After the command completes, you will find the standalone `.exe` file in the `dist` directory.

## Disclaimer

This script is provided for educational and research purposes only. The author is not responsible for any misuse or damage caused by this software. Unauthorized access to computer systems and data is illegal. Use this script responsibly and only on systems you have explicit permission to test.
