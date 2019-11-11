# Moderation

WriteFreely offers a growing number of tools to moderate your community of writers.

## Registration

You can choose who to admit or leave out of your community as a good first step toward building a healthy online space. These are the types of registration you can choose from, from more open to closed:

**Open Registration**. Anyone who comes to your site can join. This is perfect for getting as many people as possible to join your community. To use this mode, enable the "Open Registrations" option in your Admin dashboard, or set `open_registration = true` in your configuration file.

**Invite-only**. This mode requires new users to have a special invite link to sign up. Admins may choose who can generate these invite links by going to their Dashboard and selecting "Users" or "Admins" for the "Allow sending invitations by" option.

 **Closed Registration**. In this mode, no new users may join your instance. This occurs when invites are disabled (in the Dashboard, the "No one" option is selected, or `user_invites = ` (blank) in the configuration file).

## Silencing Users

If you have registered users who are causing problems on your instance, you can **silence** their account. This hides all of their posts and blogs (and any indication they ever existed) from the wider web, while keeping their data intact, in case you ever want to restore visibility to their account.
