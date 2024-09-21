# RDP-Stealer - Capture RDP Credential 

## Overview
**RDP-Stealer** is a tool designed to capture clear-text credentials during Remote Desktop Protocol (RDP) sessions by injecting a DLL into the `mstsc.exe` process. This method enables credential extraction when a user logs in via RDP, facilitating lateral movement within a network.

This tool uses `rdptheif.dll` for the injection process and requires `inject.exe` to be executed **before** a user logs in. The captured credentials are stored for later retrieval.

## Features
- Captures clear-text credentials during RDP login.
- Injects `rdptheif.dll` into the target RDP session using the `inject.exe` tool.
- Continuously monitors active sessions to capture credentials in real-time.
- Efficiently captures credentials compared to verbose keylogger outputs.

## Prerequisites
- Ensure that the `rdptheaf.dll` file is available on the target machine in the correct directory (e.g., `C:\Tools\RdpThief.dll`).
- Update the path in the `inject.cs` file to reflect the correct location of the DLL.
- Compile `inject.exe` with the appropriate architecture for the target machine.
  
## How it Works
When a user (e.g., `dave`) logs into a server (`app01`), and another user (e.g., `admin`) initiates an RDP session from `app01` to a domain controller (`dc01`), **RDP-Stealer** intercepts the credentials of the admin in clear text. This is achieved by injecting `rdptheif.dll` into the RDP process (`mstsc.exe`) and monitoring the session for credential data.

Unlike a generic keylogger that captures every keystroke and produces large, verbose outputs, **RDP-Stealer** focuses specifically on credential capture, providing clear and actionable results.

## Setup & Usage

1. **Prepare the Files**  
   - Ensure `rdptheif.dll` is placed in the correct path (update the path in `inject.cs` if necessary).
   - Compile the `inject.cs` file from the repository with the correct architecture (32-bit or 64-bit).

2. **Deploy & Run the Tool**  
   - Log into the server `appsrv01` as `dave`.
   - Copy the compiled executable (`Inject.exe`) to `C:\Tools\`.
   - Start an RDP session using `mstsc.exe` (Remote Desktop Connection).
   - Run `Inject.exe` **before** the `admin` user logs in via RDP.
   - Use `mstsc` to log in to `dc01` as the `admin` user.

3. **Extract Captured Credentials**  
   The captured credentials will be stored in a binary file (`data.bin`) within the temporary folder. You can view the contents of the file by running:

   ```bash
   type C:\Users\dave\AppData\Local\Temp\6\data.bin
   ```

## Example Workflow

1. Log in to `appsrv01` as `dave`.
2. Compile and copy `Inject.exe` to the target machine's `C:\Tools\` directory.
3. Run `Inject.exe` on `appsrv01`.
4. Open `mstsc.exe` and log in to `dc01` as the `admin` user.
5. The tool will capture the `admin` user's credentials and store them in `data.bin`.

## Notes
- Always run `Inject.exe` **before** the user logs in for proper credential capture.
- This tool works for live RDP sessions and will monitor active or new sessions to extract credentials.
- Ensure you have administrative privileges to perform process injection and capture credentials.

## Limitations
- Requires administrative privileges on the target machine.
- Injection must occur before the RDP session starts to capture credentials. So if you are doing pentesting then start this exe in mostly used workstsions where high valued target may log in and check next day.

## Conclusion
**RDP-Stealer** is a specialized tool for capturing clear-text credentials during RDP sessions using DLL injection. It simplifies the process of obtaining credentials in real-time, allowing security testers and ethical hackers to assess the security of RDP implementations. Ensure proper configuration of the necessary files and enjoy clean, actionable credential dumps during live sessions.
