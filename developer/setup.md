# Development Setup

Ready to hack on your site? Here's a quick overview.

## Prerequisites

* [Go 1.13+](https://golang.org/dl/)
* [Node.js](https://nodejs.org/en/download/)

## Setting up

After installing Go and Node.js, run the following commands to build, configure, and run the application.

```bash
go get -d github.com/writeas/writefreely/cmd/writefreely

cd $GOPATH/src/github.com/writeas/writefreely

make build   # Compile the application
make install # Config, generate keys, setup database, install LESS compiler
make run     # Run the application
```
