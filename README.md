# FooTube — C++ Recommendation Server

A multithreaded TCP server in C++ that powers video recommendations for **FooTube**, a YouTube-style video platform built by a team of three as a Bar-Ilan University Advanced Programming project.

It tracks which users watched which videos and recommends new videos using **collaborative filtering**: videos watched by people who watched the same things you did, ranked by how often they appear.

## How it works

The server listens on port **5555** and starts a new thread for every client connection. Each connection speaks a simple text protocol:

| Command | Example | Response |
|---------|---------|----------|
| `UPDATE_VIEW <userId> <videoId>` | `UPDATE_VIEW u1 v42` | Confirmation that the view was recorded |
| `GET_RECOMMENDATIONS <userId>` | `GET_RECOMMENDATIONS u1` | Space-separated video IDs, best match first |

The recommendation algorithm:

1. Take every video the user has watched.
2. For each of those videos, find the other users who watched it.
3. Score every video those users watched that this user hasn't seen yet, one point per co-viewer.
4. Return the videos sorted by score.

## The FooTube project

| Repository | What it is |
|------------|------------|
| [FooTube-Server](https://github.com/elayben/FooTube-Server) | Node.js + Express REST API with MongoDB |
| [FooTube-Web](https://github.com/elayben/FooTube-Web) | React web client |
| [FooTube-Android](https://github.com/elayben/FooTube-Android) | Native Android client (Java) |
| **FooTube-CPP-Server** (this repo) | Multithreaded C++ TCP server for video recommendations |

## Getting started

**Prerequisites:** Linux or WSL, a C++14 compiler, and CMake 3.16+.

```bash
git clone https://github.com/elayben/FooTube-CPP-Server.git
cd FooTube-CPP-Server
cmake -S . -B build
cmake --build build

./build/Server    # → Server is listening on port 5555
```

To try it by hand, open a second terminal and talk to the server with `nc`:

```bash
nc 127.0.0.1 5555
UPDATE_VIEW alice v1
UPDATE_VIEW bob v1
UPDATE_VIEW bob v2
GET_RECOMMENDATIONS alice      # → v2
```

`Client.cpp` is a minimal connectivity check: it connects to `127.0.0.1:5555`, sends one message and prints the reply.

## Screenshots

The [`wiki/`](wiki) folder has screenshots of the full FooTube system running end to end.

<p align="center">
  <img src="wiki/Screenshot_11.png" alt="FooTube home feed" width="720">
</p>

## Team

Built by a team of three, with work split by feature and tracked in Jira. [Elay Ben Yehoshua](https://github.com/elayben) was one of the three developers.
