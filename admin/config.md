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
| `default_visibility` | The default visibility setting for newly-created blogs. Valid choices: `unlisted` (default), `public`, or `private` | public |

## OAuth

There are several possible OAuth configuration blocks for different implementations.

### Generic OAuth

This is for the most general case that should work with many spec-compliant OAuth providers.

| Field | Description | Example value |
| ----- | ----------- | ------- |
| `client_id` | The client ID, or client key, associated with WriteFreely in the OAuth provider application. | _(a long string of characters)_ |
| `client_secret` | The client secret associated with WriteFreely in the OAuth provider application. | _(a long string of characters)_ |
| `host` | The base url of the OAuth provider application, including the protocol. | https://example.com |
| `display_name` | The human-readable name of the OAuth service that appears on the login button, will appear as "Log in with [display_name]". | _(name of the application)_ |
| `callback_proxy` | The url of an inbound proxy that sits in front of the default `/oauth/callback/generic` endpoint. Use if you want the OAuth callback to be somewhere other than that generic location. Default is blank.| https://example.com/whatever/path |
| `callback_proxy_api` | The url of an outbound proxy to send your OAuth requests through. Default is blank. | https://my-proxy.example.com |
| `token_endpoint` | The API endpoint of the OAuth provider implementation to obtain an access token by presenting an authorization grant or refresh token. This is a fragment of a url, appended to `host` as described above. | /oauth/token |
| `inspect_endpoint` | The API endpoint of the OAuth provider that returns basic user info given their authentication information. This is a fragment of a url, appended to `host` as described above. | /oauth/userinfo |
| `auth_endpoint` | The API endpoint of the OAuth provider that returns an authorization grant. This is a fragment of a url, appended to `host` as described above. | public |
| `scope` | A scope or set of scopes required by some OAuth providers. This will usually be blank in this config file, and is set to "read_user" by default. | read_user |
| `allow_disconnect` | Whether or not an individual user is allowed to disconnect this OAuth provider from their account. | false |
