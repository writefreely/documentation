# Admin Commands

The following application flags allow administrators to perform certain actions on their instance, including installing, upgrading, or maintaining it.

## Options

These options can be used in conjunction with any other flags.

| Flag | Description |
| ---- | ----------- |
| `-c [filename]` | Config file to use with any other operation |
| `--debug` | Output debug information in application logs |

## Setup

Use these flags to perform certain actions as part of the setup process.

| Flag | Description | Interactive? |
| ---- | ----------- | ------------ |
| `--config` | Start the configuration process | Yes |
| `--gen-keys` | Generate encryption keys | No |
| `--init-db` | Initialize the database by creating the necessary tables | No |

## Upgrade

These flags assist with upgrading an instance.

| Flag | Description |
| ---- | ----------- |
| `--migrate` | Migrate database schema to the latest version |

## User administraction

Use these flags to perform actions around users.

| Flag | Description | Interactive? |
| ---- | ----------- | ------------ |
| `--create-admin [username]:[password]` | Create an admin user in the database. Fails if admin already exists. | No |
| `--create-user [username]:[password]` | Create a regular user in the database. Fails if no admin user exists yet. | No |
| `--reset-pass [username]` | Reset the given user's password | Yes |

## Miscellaneous

| Flag | Description |
| ---- | ----------- |
| `-v` | Print WriteFreely version information |
