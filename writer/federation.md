# Federation

WriteFreely supports federation via ActivityPub, a protocol spoken by popular platforms like [Mastodon](https://joinmastodon.org) (an alternative to Twitter). This means that other people can directly follow your blog from the decentralized social network known as the "fediverse," if your WriteFreely admin has enabled federation.

## Finding your handle

To find your handle, go into your blog's settings. Under _URL_ will be your blog's fediverse handle, following the format `@blog@instance.com`.

## Following your blog

To follow your blog, open Mastodon, Pleroma, or your other favorite ActivityPub-powered platform, and search for the fediverse handle from your settings page. (**Note**: you can also search for the blog URL, instead of the handle.) Finally, click the "Follow" button. This will make sure you receive future posts in your timeline, where you can then _favorite_ or _boost_ them to your followers.

## Note about changing your username

If you change your username, a new fediverse handle will be created. This means the previous fediverse handle will stop receiving new posts. Anyone who was following your previous fediverse handle will have to follow the new one in order to receive your most recent posts.

## Demo

[Watch this video](https://video.writeas.org/videos/watch/cc55e615-d204-417c-9575-7b57674cc6f3) to see this feature in action.

## ActivityPub Mentions

With federation enabled, you can mention users of Mastodon, Pleroma, and other ActivityPub platforms from your blog. To mention someone, insert `@handle@their.instance` in a blog post. Once published, the fediverse user will see your post mentioning them in their notifications.
