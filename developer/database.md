# Database Values

The following is a description of the various database tables and attributes.  (This document is not yet complete).

## `accesstokens`
Generated user access tokens for accessing the API.

## `appcontent`
Instance-level dynamic content, usually created via the admin dashboard, such as the About and Privacy pages.

## `appmigrations`
All database migrations that have been run on this database.

## `collectionattributes`
Used for additional properties on collections.

## `collectionkeys`
Public / private keypairs for all collections on the instance. Used to sign ActivityPub / fediverse requests.

## `collectionpasswords`
Salted and hashed passwords for password-protected collections.

## `collectionredirects`
Data about former `alias`es for collections that have had them changed, so that the old `alias` redirects visitors to the new one.

## `collections`
Table that contains collections (i.e. "Blogs").

* `id`: *int(6)*. **Primary Key**.  Blog id.  Instance specific.

* `alias`: *varchar(100)*.  Blog identifier based on the blog’s title when created.

* `title`: *varchar(255*. **Cannot be null**.  Blog name.

* `description`: *varchar(160)*. **Cannot be null**.  User defined description of blog.

* `style_sheet`: *text*.  CSS stylesheet data goes here.

* `script`: *text*.  **Currently unused.**

* `format`: *varchar(8)*.  User defined format (`blog`, `novel`, or `notebook`).

* `privacy`: *tinyint(1)*. **Cannot be null**. 0=Unlisted. 2=Private. 4=Password Protected.

* `owner_id`: *int(6)*.  **Cannot be null**.  The id of the user that published this collection.

* `view_count`: *int(6)*.  **Cannot be null**.  How many views this collection has.

## `posts`
Table that contains data for posts within collections.

* `id`: **Primary Key.** *Char(16)*.  Randomly generated id.

* `slug`: *varchar(100)*.  Identifier for post.  Smartly generated based on post content, favoring: Title > Text > Id.

* `modify_token`: *char(32)*.

* `text_appearance`: *char(4)*. **Cannot be null**.  Font class (Serif=`norm`, Sans-serif=`sans`, Monospace=`wrap`)

* `language`: *char2*. The post’s language.

* `rtl`: *tinyint(1)*. Value is 0 if the post is written left-to-right, 1 if written right-to-left (e.g. Arabic), and null if user never explicitly provided this value.

* `privacy`: **Not currently used.**

* `owner_id`: *int(6)*.  Id of the author.  Id is instance-specific.

* `collection_id`: *int(6)*.  Id of the blog.  Id is instance-specific. Null if post is a Draft.

* `pinned_position`: *tinyint(1)*.  The order the post is "pinned" (zero-indexed).  Null if not pinned.

* `created`: *timestamp*.  **Cannot be null**.  Time and date of first publish

* `updated`: *timestamp*.  **Cannot be null**.  Time and date of last edit

* `view_count`: *int(6)*.  **Cannot be null**.  How many views this post has.

* `title`: *varchar(160)*.  **Cannot be null**.  Title, if author created one (may be empty string).

* `content`: *text*.  **Cannot be null**.  The content of the post in plain text.

## `remotefollows`
Data about which remote, i.e. ActivityPub / fediverse, users are following which collections.

## `remoteuserkeys`
Public keys of all remote users.

## `remoteusers`
Data needed to communicate with remote users.

## `userattributes`
Additional attributes on users.

## `userinvites`
User invite codes and metadata.

## `users`
Table that contains user data.

* `id`: *int(6)*. **Primary Key**. User id.  Instance specific.

* `username`: *varchar(100)*.  **Cannot be null**.  Chosen username.

* `password`: *char(60)*.  **Cannot be null**.  Salted and hashed password.

* `email`: *varbinary(255)*.  Encrypted email address. (null if not given by user).

* `created`: *datetime*.  **Cannot be null**.  Date and time account was created.

## `usersinvited`
Data about which users were brought in by which user-invite.
