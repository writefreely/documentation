# Development Setup

Ready to hack on your site? Here's a quick overview.

## Prerequisites

* [Go 1.13+](https://golang.org/dl/)
* [Node.js](https://nodejs.org/en/download/)

## Quick start

After installing Go and Node.js, run the following commands to build, configure, and run the application.

```bash
go get -d github.com/writefreely/writefreely/cmd/writefreely

cd $GOPATH/src/github.com/writefreely/writefreely

make build   # Compile the application
make install # Config, generate keys, setup database, install LESS compiler
make run     # Run the application
```

## Detailed steps

Use the following steps to build WriteFreely without Make. First, get the code:

```bash
git clone https://github.com/writefreely/writefreely.git
cd writefreely
```

Finally, build the `writefreely` binary with SQLite support. (Remove `-tags='sqlite'` if you don't need SQLite support.)

```bash
go build -v -tags='sqlite' ./cmd/writefreely/
```

You can now run WriteFreely! But you'll need one more step to generate some assets and successfully run an instance.

### Building site assets

You'll need Node.js and LESS installed to generate WriteFreely static assets. Install LESS:

```bash
npm install less less-plugin-clean-css
```

Next, compile all stylesheets from the `less` directory, creating them in the `static/css/` directory:

```bash
cd less
LESSC=../node_modules/less/bin/lessc
CSSDIR=../static/css
$LESSC app.less --clean-css="--s1 --advanced" ${CSSDIR}/write.css
$LESSC fonts.less --clean-css="--s1 --advanced" ${CSSDIR}/fonts.css
$LESSC icons.less --clean-css="--s1 --advanced" ${CSSDIR}/icons.css
```

Now you can run and distribute WriteFreely! 🎉

Beyond this, you'll initialize your individual instance.

### Application initiation

With your build complete, create a configuration file, encryption keys, and initialize your database.

#### Config file

Most users will want the interactive configuration process. Run with:

```bash
writefreely config start
```

For other cases where you only need a blank configuration, you can non-interactively generate `config.ini` with this command:

```bash
writefreely config generate
```

#### Encryption keys

Generate encryption keys needed for storing user sessions and private data:

```bash
writefreely keys generate
```

#### Database

With your instance configured for a database, run the following command to create the necessary tables:

```bash
writefreely db init
```

### Run WriteFreely

```bash
writefreely
```
