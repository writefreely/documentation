# Admin Commands

The following application commands allow administrators to perform certain actions on their instance, including installing, upgrading, or maintaining it.

## Options

These options can be used in conjunction with any other flags.

| Flag | Description |
| ---- | ----------- |
| `-c [filename]` | Config file to use with any other operation |
| `--debug` | Output debug information in application logs |
| `-h` | Output help for any command |

## Setup

Use these flags to perform certain actions as part of the setup process.

| Command | Description | Interactive? |
| ------- | ----------- | ------------ |
| `config start` | Start the configuration process | Yes |
| `keys generate` | Generate encryption keys | No |
| `db init` | Initialize the database by creating the necessary tables | No |

For example, run these commands in order to set up your instance:

```
writefreely config start
writefreely keys generate
```

### Setup options

#### `--config --sections="..."`

You can optionally choose which configuration sections to walk through during the configuration process with the `--sections` flag. Values are space-separated and must be one of the following:

* `app`
* `db`
* `server`

Example usage:

```
writefreely --config --sections="app db server"
```

## Upgrade

These flags assist with upgrading an instance.

| Command | Description |
| ------- | ----------- |
| `db migrate` | Migrate database schema to the latest version |

## User administration

Use these flags to perform actions around users.

| Command | Description | Interactive? |
| ------- | ----------- | ------------ |
| `user create --admin [username]:[password]` | Create an admin user in the database. Fails if admin already exists. | No |
| `user create [username]:[password]` | Create a regular user in the database. Fails if no admin user exists yet. | No |
| `user reset-pass [username]` | Reset the given user's password | Yes |
| `user delete [username]` | Delete the given user, after confirming interactively | Yes |

## Miscellaneous

| Command | Description |
| ------- | ----------- |
| `-v` | Print WriteFreely version information |
