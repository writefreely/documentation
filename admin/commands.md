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

## Mass user management

Use these commands to review and act on many users at once. Each command accepts the below Filters to narrow down which users it applies to.

| Command                   | Description                                                                                            | Interactive? |
|---------------------------|--------------------------------------------------------------------------------------------------------|--------------|
| `users list [filters]`    | List users matching the given filters. Includes admins.                                                | No           |
| `users silence [filters]` | List and silence all users matching the given filters, after confirming interactively                  | Yes          |
| `users delete [filters]`  | List and delete all users matching the given filters and their content, after confirming interactively | Yes          |

Admins are never silenced or deleted via these subcommands, no matter the filters -- they only appear in `users list`.

### Filters

Filters can be combined, and only include users that match all given conditions.

| Flag                   | Description                                              |
|------------------------|----------------------------------------------------------|
| `--since [YYYY-MM-DD]` | Include users created on or after this date              |
| `--until [YYYY-MM-DD]` | Include users created before this date                   |
| `--no-invite`          | Include users who signed up without an invite code       |
| `--no-oauth`           | Include users who did not register via an OAuth provider |
| `--max-posts [n]`      | Include users with at most `n` posts                     |

## Miscellaneous

| Command | Description |
| ------- | ----------- |
| `-v` | Print WriteFreely version information |
