# Chorus Mode

_This is an experimental feature._

Chorus Mode reconfigures the WriteFreely user interface to be optimized for the _group_, instead of the _individual_. It's perfect for writing groups and company instances, where the purpose is to share knowledge collectively, instead of on separate, distinct blogs.

### Status

This functionality is in a half-baked state, where it solves a certain need, but still needs to be fully fleshed-out. There are plenty of directions it could go in still, but we need more input and experimentation to figure out which one makes the most sense. Please [share any feedback](https://discuss.write.as/t/chorus-mode/2944) you have, if you use this feature!

## Effects

Chorus Mode changes the navigation of the instance to stay grounded in the entire instance, rather than individual blogs. For example, blogs and blog posts maintain the site-wide navigation at the top of the page, instead of featuring a single blog's personal branding there.

It overrides the `landing` config setting and makes the **Reader** the default view for all users, so they'll always land there when first arriving on the instance, whether logged in or not.

Hashtags also filter posts collectively, showing posts from _across the instance_ all using the same hashtag when clicked, instead of only showing posts from a single blog.

## Uses

### Organizations

Chorus Mode is perfect for creating an internal knowledge-sharing space within your organization. For example, the Write.as team uses Chorus Mode on their internal WriteFreely instance, where team members share proposals, strategy, and general updates with the rest of the team.

Start with these `[app]` configuration options for your organization's WriteFreely instance:

| Field | Value |
| ----- | ----- |
| `single_user` | `false` |
| `chorus` | `true` |
| `private` | `true` |
