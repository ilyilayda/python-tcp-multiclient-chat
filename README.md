# Python TCP Multi-Client Chat Application

A desktop chat application developed in Python to demonstrate TCP socket communication, client–server architecture, and multithreaded connection handling. The client provides a graphical interface using Tkinter and CustomTkinter.

> **Project status:** Educational prototype. The existing source code is provided without functional changes. See Known Limitations before running.

## Features

- TCP-based client–server messaging
- Username-based entry to the chat
- Message broadcasting to connected clients
- Thread-based handling of concurrent client connections
- Desktop chat interface with send button and Enter shortcut
- Light/dark appearance and interface scaling controls

## Technologies

- Python 3
- `socket` (standard library)
- `threading` (standard library)
- `tkinter` (standard library, typically bundled with Python)
- `customtkinter` (external package)

## Project structure

```text
python-tcp-multiclient-chat/
├── server.py                # TCP server and message broadcasting
├── client.py                # GUI client and message handling
├── requirements.txt         # Third-party dependency
├── .gitignore
├── README.md
└── docs/
    └── Project_Report.docx
```

## Installation and usage

1. Install Python 3 with Tkinter support.
2. Install the GUI dependency:

   ```bash
   pip install -r requirements.txt
   ```

3. Configure `SERVER_HOST` in `server.py` and `HOST` in `client.py`:
   - For both processes on the same computer, use `127.0.0.1` in both files.
   - For different computers on a LAN, use the server computer's reachable local IPv4 address in both files and allow inbound TCP port `1234` in the firewall.
4. Run the server first:

   ```bash
   python server.py
   ```

5. Open one or more **separate terminals** and start the client in each:

   ```bash
   python client.py
   ```

6. Enter a username, connect, and exchange messages.

## Architecture

```text
Client A ──TCP──┐
Client B ──TCP──┼──> Server (port 1234) ──broadcast──> Connected clients
Client C ──TCP──┘
```

The server accepts incoming sockets, associates usernames with client sockets, and starts a worker thread per connection. Each client uses a listener thread to read messages and a GUI to compose and display them.

## Known limitations

- The IP `XX.XX.XX.XXX` is hardcoded in the original files and may not exist on other machines.
- The client can attempt to use its socket after an unsuccessful connection.
- The server does not correctly terminate the receive loop and remove disconnected clients; an empty receive can cause repeated logging, and later broadcasts may fail.
- TCP data is treated as one full message per `recv()`. TCP does not guarantee message boundaries, so messages can be split or combined.
- The client splits messages on `!`, potentially losing or misparsing message content containing this character.
- Tkinter widgets are manipulated from a background listener thread and the message bubbles are `Label` widgets placed inside a `Text` widget, which can cause GUI errors; proper use requires thread-safe scheduling and a supported text-widget insertion strategy.
- Message transport is unencrypted; the application does not implement authentication or persistent history.
- A declared backlog of `10` (`listen(MAX_CLIENTS)`) is not a strict limit on the total number of connected users.

These issues should be addressed and the application tested before representing it as production-ready.

## Potential improvements

Thread-safe GUI updates, robust disconnect handling, length-prefixed or newline-delimited message framing, chat logs, authentication, encryption, unit tests, and packaging.

## Report

See [`docs/Project_Report.docx`](docs/Project_Report.docx) for project goals, implementation, architecture, limitations and development recommendations.
