# Scheduling Posts

WriteFreely supports scheduling posts to be published at a future date.

To schedule a post from the web application:

1. Publish a post as a _Draft_
1. At the top of the page, click _Edit_
1. Notice the new "i" icon at the top of the editor; click that
1. Change the "Created" time to a date in the future, and save your settings
1. Finally, go to your Drafts page
1. Click "move to _your blog_"

**Advanced.** To schedule a post from the API:

1. When [publishing a blog post](https://developers.write.as/docs/api/#publish-a-collection-post), include the `created` property

## Notes

* Scheduled posts are hidden from your blog until the _published_ date has passed
* Scheduled posts won't be sent out via federation / ActivityPub (to be fixed with [T567](https://writefreely.org/tasks/567))
