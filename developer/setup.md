# Development Setup

Ready to hack on your site? Here's a quick overview.

## Prerequisites

* [Go 1.10+](https://golang.org/dl/)
* [Node.js](https://nodejs.org/en/download/)

## Setting up

Run these commands to get set up.

```bash
go get -d github.com/writeas/writefreely/cmd/writefreely

# If running Go 1.11+
export GO111MODULE=on

make build   # Compile the application
./cmd/writefreely/writefreely --config # Create configuration file
make install # Generates encryption keys; installs LESS compiler
make run     # Runs the application
```
