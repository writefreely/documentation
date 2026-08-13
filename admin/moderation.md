# Moderation

WriteFreely offers a growing number of tools to moderate your community of writers.

## Registration

You can choose who to admit or leave out of your community as a good first step toward building a healthy online space. These are the types of registration you can choose from, from more open to closed:

**Open Registration**. Anyone who comes to your site can join. This is perfect for getting as many people as possible to join your community. To use this mode, enable the "Open Registrations" option in your Admin dashboard, or set `open_registration = true` in your configuration file.

**Invite-only**. This mode requires new users to have a special invite link to sign up. Admins may choose who can generate these invite links by going to their Dashboard and selecting "Users" or "Admins" for the "Allow sending invitations by" option.

 **Closed Registration**. In this mode, no new users may join your instance. This occurs when invites are disabled (in the Dashboard, the "No one" option is selected, or `user_invites = ` (blank) in the configuration file).

## Silencing Users

If you have registered users who are causing problems on your instance, you can **silence** their account. This hides all of their posts and blogs (and any indication they ever existed) from the wider web, while keeping their data intact, in case you ever want to restore visibility to their account.

## Mass User Moderation

Sometimes you need to act on many users at once — for example, cleaning up a wave of spam signups that slipped through, even on a closed instance. From the command line, WriteFreely lets you find these accounts by common traits and then moderate them together.

The flow has two steps. First, use `users list` with one or more filters to find the accounts you're interested in and review them. Filters let you narrow down by details like when an account was created, whether it signed up with an invite code or through OAuth, and how many posts it has.

```
writefreely users list --since 2026-07-01 --no-invite --no-oauth --max-posts 0
```

Once you're confident the list is right, run the same filters through `users silence` or `users delete` to act on every matching account at once. Silencing hides the accounts (and is reversible), while deleting removes them and their content for good. Both ask for confirmation first, and neither will ever touch an admin account.

```
writefreely users delete --since 2026-07-01 --no-invite --no-oauth --max-posts 0
```

For the full list of filters and other options, see the [admin commands reference](https://writefreely.org/docs/latest/admin/commands#mass-user-management).
