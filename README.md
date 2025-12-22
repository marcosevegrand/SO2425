# Document Indexing and Search Service

This project was developed for the Operating Systems course, part of the 2nd year of the Bachelor's degree in Software Engineering at the University of Minho, during the 2024/2025 academic year.

The system implements a distributed client-server service for the management, indexing, and searching of document metadata and text content, utilizing inter-process communication (IPC) mechanisms and concurrency.

## Authors (Group 1)

* Marco Rocha Ferreira - A106857
* Marco Rafael Vieira Sevegrand - A106807
* Nuno Henrique Macedo Rebelo - A107372

## Project Overview

The service allows users to interact with a central server to index documents and perform fast searches. The architecture is based on:

1. Server (dserver): Manages persistent storage, the memory cache, and coordinates search and line counting operations.
2. Client (dclient): Command-line interface that sends requests to the server.
3. Protocol: Communication via Named Pipes (FIFOs) using structured messages.

## Key Features

* Metadata Indexing: Registration of title, authors, year, and file path.
* Disk Persistence: Storage in an index file to maintain data after server shutdown.
* Cache Management: Implementation of a configurable LRU (Least Recently Used) cache to optimize access times.
* Concurrent Search: Keyword searching using multiple processes (fork) to maximize performance on multi-core systems.
* Occurrence Counting: Counting lines containing a specific keyword within a specific document.

## System Requirements

* Operating System: Linux / Unix-like
* Compiler: GCC
* Dependencies: GLib 2.0 (libglib2.0-dev)

## Compilation Instructions

The project includes a Makefile to manage binaries. To compile, run:
`make`

To remove object files and binaries:
`make clean`

## Usage Guide

### Running the Server

The server must be started by specifying the directory where the documents are located and, optionally, the cache size:
`./bin/dserver <documents_directory> [cache_size]`

### Client Commands

The client interacts with the server through the following flags:

* Add document:
`./bin/dclient -a "Title" "Author" Year "Path"`
* Consult metadata:
`./bin/dclient -c <document_id>`
* Delete document:
`./bin/dclient -d <document_id>`
* Count lines with keyword:
`./bin/dclient -l <document_id> <keyword>`
* Global search (concurrent):
`./bin/dclient -s <keyword> [number_of_processes]`
* Shutdown server:
`./bin/dclient -f`

## Project Structure

* src/: Source code for the client, server, and common protocols.
* include/: Header files (.h).
* scripts/: Test scripts and automation tools for data loading (e.g., Gcatalog).
* bin/: Binaries resulting from compilation.
* obj/: Object files.

## Technical Details

* IPC: Use of FIFOs in tmp/ for global requests and PID-specific responses.
* Process Management: Use of fork() and waitpid() to avoid zombie processes and ensure parallel execution of heavy searches.
* Search: Integration with the system's grep tool for text search operations.

Submission Date: May 5th, 2025
