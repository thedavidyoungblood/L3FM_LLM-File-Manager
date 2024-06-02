# LlamaFS

<img src="electron-react-app/assets/llama_fs.png" width="30%" />

## Inspiration

[Watch the explainer video](https://x.com/AlexReibman/status/1789895425828204553)

Open your `~/Downloads` directory. Or your Desktop. It's probably a mess...

> There are only two hard things in Computer Science: cache invalidation and **naming things**.

## What it does

LlamaFS is a self-organizing file manager. It automatically renames and organizes your files based on their contents and well-known conventions (e.g., time). It supports many kinds of file, and even images (through Moondream) and audio (through Whisper).

LlamaFS runs in two "modes" - as a batch job (batch mode), and an interactive daemon (watch mode).

In batch mode, you can send a directory to LlamaFS, and it will return a suggested file structure and organize your files.

In watch mode, LlamaFS starts a daemon that watches your directory. It intercepts all filesystem operations, updates i and uses your most recent edits in context to proactively learn and how, so you don't 
learns predict how you rename file. e.g. if you create a folder for 2023 tax documents, and start moving 1-3 file in it, LlamaFS will automatically creates, and move the right!

Uhh... Sending all my personal files to an API provider?! No thank you!

It also has a toggle for "incognito mode", allowing you route every request through Ollama instead of Groq. Since they use the same Llama 3 model, the perform identically.

## How we built it

We built LlamaFS on a Python backend, leveraging the Llama3 model through Groq for file content summarization and tree structuring. For local processing, we integrated Ollama running the same model to ensure privacy in incognito mode. The frontend is crafted with Electron, providing a sleek, user-friendly interface that allows users to interact with the suggested file structures before finalizing changes.

- **It's extremely fast!** (for LLM standards)! Most file ops are processed in <500ms in watch mode (benchmarked by [AgentOps](https://agentops.ai/?utm_source=llama-fs)). This is because of our smart caching, that selectively rewrites sections of the index based on the minimum nessecary filesystem diff. And of course, Groq's super fast inference API. 😉

- **It's immediately useful** - It's very low friction to use, and a problem almost everyone has. We started using it ourselves on this project (very Meta)


## What's next for LlamaFS

- Find and remove old/unused files
- We have some really cool ideas for - filesystem diffs are hard...


## Installation

### Prerequisites

Before installing, ensure you have the following requirements:
- Python 3.10 or higher
- pip (Python package installer)

### Installing

To install the project, follow these steps:
1. Clone the repository:
   ```bash
   git clone https://github.com/iyaja/llama-fs.git
   ```

2. Navigate to the project directory:
    ```bash
    cd llama-fs
    ```

3. Install requirements
   ```bash
   pip install -r requirements.txt
   ```
4. (Optional) Install moondream if you want to use the incognito mode
    ```bash
    ollama pull moondream
    ```
## Usage

To serve the application locally using FastAPI, run the command
   ```bash
   fastapi dev server.py
   ```

This will run the server by default on port 8000. The API can be queried using a curl command, and passing in the file path as the argument. For example, on the Downloads folder
   ```bash
   curl -X POST http://127.0.0.1:8000 \
    -H "Content-Type: application/json" \
    -d '{"path": "/Users/<username>/Downloads/", "instruction": "string", "incognito": false}'
   ```

---

---
Title: Comprehensive Guide to Launching FastAPI Server and Browsers with Tor on Different Operating Systems
Description: A detailed guide on how to create shortcuts for launching a FastAPI server and various browsers with Tor functionality on macOS, Linux, and Windows.
Date: 2023-06-02
Tags:
 - "#fastapi"
 - "#browser-launch"
 - "#tor"
 - "#cross-platform"
---

# Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [macOS](#macos)
   - [Chrome](#chrome-macos)
   - [Brave](#brave-macos)
   - [Safari](#safari-macos)
4. [Linux](#linux)
   - [Chrome](#chrome-linux)
   - [Brave](#brave-linux)
5. [Windows](#windows)
   - [Chrome](#chrome-windows)
   - [Brave](#brave-windows)
6. [Troubleshooting](#troubleshooting)
7. [Conclusion](#conclusion)

## Introduction <a name="introduction"></a>

This guide provides step-by-step instructions for creating shortcuts to launch a FastAPI server and various browsers with Tor functionality on different operating systems. The supported operating systems are macOS, Linux, and Windows, and the browsers covered are Chrome, Brave, and Safari (macOS only).

## Prerequisites <a name="prerequisites"></a>

Before proceeding with the guide, ensure that you have the following prerequisites:

- Python 3.10 or higher installed on your system.
- The necessary dependencies for running the FastAPI server (`fastapi`, `uvicorn`, etc.) installed in your Python environment.
- The browsers (Chrome, Brave, Safari) installed on your respective operating system.

## macOS <a name="macos"></a>

### Chrome <a name="chrome-macos"></a>

1. Create a shell script named `launch_chrome_tor.sh` with the following content:
   ```bash
   #!/bin/bash
   
   /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --incognito --proxy-server="socks5://localhost:9150" &
   
   cd "/path/to/your/fastapi/project"
   fastapi run server:app
   ```
   Replace `/path/to/your/fastapi/project` with the actual path to your FastAPI project directory.

2. Make the shell script executable:
   ```bash
   chmod +x launch_chrome_tor.sh
   ```

3. Create an Automator application:
   - Open Automator and choose "Application" as the type of document.
   - Drag the "Run Shell Script" action to the workflow area.
   - Set the shell to `/bin/bash`.
   - Enter the following command in the script area:
     ```bash
     /path/to/launch_chrome_tor.sh
     ```
     Replace `/path/to/launch_chrome_tor.sh` with the actual path to your shell script.
   - Save the Automator application with a meaningful name (e.g., "Chrome with Tor").

4. Launch the Automator application to start Chrome with Tor and the FastAPI server.

### Brave <a name="brave-macos"></a>

1. Create a shell script named `launch_brave_tor.sh` with the following content:
   ```bash
   #!/bin/bash
   
   /Applications/Brave\ Browser.app/Contents/MacOS/Brave\ Browser --incognito --tor &
   
   cd "/path/to/your/fastapi/project"
   fastapi run server:app
   ```
   Replace `/path/to/your/fastapi/project` with the actual path to your FastAPI project directory.

2. Make the shell script executable:
   ```bash
   chmod +x launch_brave_tor.sh
   ```

3. Create an Automator application:
   - Open Automator and choose "Application" as the type of document.
   - Drag the "Run Shell Script" action to the workflow area.
   - Set the shell to `/bin/bash`.
   - Enter the following command in the script area:
     ```bash
     /path/to/launch_brave_tor.sh
     ```
     Replace `/path/to/launch_brave_tor.sh` with the actual path to your shell script.
   - Save the Automator application with a meaningful name (e.g., "Brave with Tor").

4. Launch the Automator application to start Brave with Tor and the FastAPI server.

### Safari <a name="safari-macos"></a>

1. Create a shell script named `launch_safari_fastapi.sh` with the following content:
   ```bash
   #!/bin/bash
   
   open -a Safari http://127.0.0.1:8000 &
   
   cd "/path/to/your/fastapi/project"
   fastapi run server:app
   ```
   Replace `/path/to/your/fastapi/project` with the actual path to your FastAPI project directory.

2. Make the shell script executable:
   ```bash
   chmod +x launch_safari_fastapi.sh
   ```

3. Create an Automator application:
   - Open Automator and choose "Application" as the type of document.
   - Drag the "Run Shell Script" action to the workflow area.
   - Set the shell to `/bin/bash`.
   - Enter the following command in the script area:
     ```bash
     /path/to/launch_safari_fastapi.sh
     ```
     Replace `/path/to/launch_safari_fastapi.sh` with the actual path to your shell script.
   - Save the Automator application with a meaningful name (e.g., "Safari with FastAPI").

4. Launch the Automator application to start Safari and the FastAPI server.

## Linux <a name="linux"></a>

### Chrome <a name="chrome-linux"></a>

1. Create a shell script named `launch_chrome_tor.sh` with the following content:
   ```bash
   #!/bin/bash
   
   google-chrome --incognito --proxy-server="socks5://localhost:9150" &
   
   cd "/path/to/your/fastapi/project"
   fastapi run server:app
   ```
   Replace `/path/to/your/fastapi/project` with the actual path to your FastAPI project directory.

2. Make the shell script executable:
   ```bash
   chmod +x launch_chrome_tor.sh
   ```

3. Create a desktop shortcut:
   - Right-click on the desktop and select "Create Launcher".
   - Enter a name for the launcher (e.g., "Chrome with Tor").
   - In the "Command" field, enter the full path to the shell script:
     ```bash
     /path/to/launch_chrome_tor.sh
     ```
     Replace `/path/to/launch_chrome_tor.sh` with the actual path to your shell script.
   - Click "Create" to save the launcher.

4. Double-click the desktop shortcut to start Chrome with Tor and the FastAPI server.

### Brave <a name="brave-linux"></a>

1. Create a shell script named `launch_brave_tor.sh` with the following content:
   ```bash
   #!/bin/bash
   
   brave-browser --incognito --tor &
   
   cd "/path/to/your/fastapi/project"
   fastapi run server:app
   ```
   Replace `/path/to/your/fastapi/project` with the actual path to your FastAPI project directory.

2. Make the shell script executable:
   ```bash
   chmod +x launch_brave_tor.sh
   ```

3. Create a desktop shortcut:
   - Right-click on the desktop and select "Create Launcher".
   - Enter a name for the launcher (e.g., "Brave with Tor").
   - In the "Command" field, enter the full path to the shell script:
     ```bash
     /path/to/launch_brave_tor.sh
     ```
     Replace `/path/to/launch_brave_tor.sh` with the actual path to your shell script.
   - Click "Create" to save the launcher.

4. Double-click the desktop shortcut to start Brave with Tor and the FastAPI server.

## Windows <a name="windows"></a>

### Chrome <a name="chrome-windows"></a>

1. Create a batch file named `launch_chrome_tor.bat` with the following content:
   ```batch
   @echo off
   start "" "C:\Program Files\Google\Chrome\Application\chrome.exe" --incognito --proxy-server="socks5://localhost:9150"
   cd /d "C:\path\to\your\fastapi\project"
   fastapi run server:app
   ```
   Replace `C:\path\to\your\fastapi\project` with the actual path to your FastAPI project directory.

2. Create a desktop shortcut:
   - Right-click on the desktop and select "New" > "Shortcut".
   - In the "Type the location of the item" field, enter the full path to the batch file:
     ```
     C:\path\to\launch_chrome_tor.bat
     ```
     Replace `C:\path\to\launch_chrome_tor.bat` with the actual path to your batch file.
   - Click "Next" and enter a name for the shortcut (e.g., "Chrome with Tor").
   - Click "Finish" to create the shortcut.

3. Double-click the desktop shortcut to start Chrome with Tor and the FastAPI server.

### Brave <a name="brave-windows"></a>

1. Create a batch file named `launch_brave_tor.bat` with the following content:
   ```batch
   @echo off
   start "" "C:\Program Files\BraveSoftware\Brave-Browser\Application\brave.exe" --incognito --tor
   cd /d "C:\path\to\your\fastapi\project"
   fastapi run server:app
   ```
   Replace `C:\path\to\your\fastapi\project` with the actual path to your FastAPI project directory.

2. Create a desktop shortcut:
   - Right-click on the desktop and select "New" > "Shortcut".
   - In the "Type the location of the item" field, enter the full path to the batch file:
     ```
     C:\path\to\launch_brave_tor.bat
     ```
     Replace `C:\path\to\launch_brave_tor.bat` with the actual path to your batch file.
   - Click "Next" and enter a name for the shortcut (e.g., "Brave with Tor").
   - Click "Finish" to create the shortcut.

3. Double-click the desktop shortcut to start Brave with Tor and the FastAPI server.

## Troubleshooting <a name="troubleshooting"></a>

- If the shortcuts don't work, ensure that the paths in the shell scripts or batch files are correct and the FastAPI server script exists in the specified location.
- Make sure you have the necessary dependencies installed for running the FastAPI server (`fastapi`, `uvicorn`, etc.) in your Python environment.
- If the browsers don't open automatically, check if the URLs or command-line arguments in the shell scripts or batch files are correct.
- Ensure that you have the required browsers installed on your system.

## Conclusion <a name="conclusion"></a>

This guide provided detailed instructions for creating shortcuts to launch a FastAPI server and various browsers with Tor functionality on macOS, Linux, and Windows. By following the steps specific to your operating system and browser, you can easily set up shortcuts to start the FastAPI server and open the browsers with enhanced privacy and anonymity.

Remember to replace the placeholder paths with the actual paths to your FastAPI project directory and the shell scripts or batch files you created.

If you encounter any issues, refer to the troubleshooting section for common solutions.

Happy browsing with Tor and FastAPI!

## List of Relevant Backlinks
- [[FastAPI Server Launch]]
- [[Browser Launch with Tor]]
- [[Cross-Platform Guide]]
- [[Shortcuts for Browsers and Servers]]

Citations:
[1] https://sync-v2.brave.com/v2
[2] https://variations.brave.com/seed
