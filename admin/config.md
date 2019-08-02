# Configuring WriteFreely

WriteFreely is configured through an `.ini` file. By default, WriteFreely will look for the configuration file `config.ini` in the current directory. However, you can supply a different file or location by running WriteFreely with the `-c [filename]` flag, for example:

```
writefreely -c /var/lib/writefreely/config.ini
```

## Server

The following fields are valid in the `[server]` section of your configuration file. They affect how the application runs.

| Field | Description | Default |
| ----- | ----------- | ------- |
| `port` | Port for the application to serve HTTP requests on | _None_ |
| `bind` | Address to bind the application to | localhost |
| `tls_cert_path` | TLS certificate path. If supplied with `tls_key_path`, requests will be served on port 443. If `autocert` is `true`, certificates and keys will be stored in the given directory. | _None_ |
| `tls_key_path` | TLS private key path. If supplied with `tls_cert_path`, requests will be served on port 443. | _None_ |
| `autocert` | Enable automatic certificate generation with Let's Encrypt. Requires `tls_cert_path` and `tls_key_path` to not be empty, and running in standalone server mode, i.e. `port` set to `443`. | `false` |
| `templates_parent_dir` | The parent directory containing the `templates` directory | _(current directory)_ |
| `static_parent_dir` | The parent directory containing the `static` directory | _(current directory)_ |
| `pages_parent_dir` | The parent directory containing the `pages` directory | _(current directory)_ |
| `keys_parent_dir` | The parent directory containing the `keys` directory | _(current directory)_ |

## Database

The following fields are valid in the `[database]` section of your configuration file. They affect how the application stores and retrieves data.

| Field | Description | Default |
| ----- | ----------- | ------- |
| `type` | Database driver type. Valid choices: `mysql` or `sqlite3` | _None_ |

These fields only apply to instances using **MySQL**.

| Field | Description | Default |
| ----- | ----------- | ------- |
| `username` | Database username | _None_ |
| `password` | Database password | _None_ |
| `database` | Database name | _None_ |
| `host` | Database hostname to connect to | localhost |
| `port` | Database host port to connect to | 3306 |

These fields only apply to instances using **SQLite**.

| Field | Description | Default |
| ----- | ----------- | ------- |
| `filename` | Database file | _None_ |

## App

The following fields are valid in the `[app]` section of your configuration file. They affect how the application functions, especially in user-facing ways.

| Field | Description | Example value |
| ----- | ----------- | ------- |
| `site_name` | Name of the website, publicly shown in several places | Our Community |
| `site_description` | Site description, shown in NodeInfo | A place to write freely. |
| `host` | Full hostname (including scheme) users will see | https://pencil.writefree.ly |
| `single_user` | Whether or not the instance is for one blog | false |
| `min_username_len` | Minimum required length of usernames | 3 |
| `federation` | Whether or not federation via ActivityPub is enabled | true |
| `public_stats` | Whether or not usage stats are made public via NodeInfo | true |

These fields can always be set, but only apply to **multi-user** instances.

| Field | Description | Example value |
| ----- | ----------- | ------- |
| `private` | When enabled, all blogs and posts will only be readable by other authenticated users on the instance. | false |
| `landing` | The default landing route for an unauthenticated user | /login |
| `open_registration` | Whether or not anyone can register via the landing page | true |
| `max_blogs` | Maximum number of blogs a single user can create under one account | 5 |
| `local_timeline` | Whether or not the instance reader (and the _Public_ option on blogs) is enabled | true |
| `user_invites` | Who is allowed to send user invites, if anyone. A blank value disables invites for all users. Valid choices: _empty_, `user`, or `admin` | user |
