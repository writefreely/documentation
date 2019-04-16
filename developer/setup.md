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
make install # Config, generate keys, setup database, install LESS compiler
make run     # Run the application
```
