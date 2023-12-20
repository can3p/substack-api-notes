# Notes on substack API

## Why?

Substack is a great platform for writing, they've done a fantastic amount of things in the way others should have years ago. The only thing that's missing for me is a cli client which would allow me to work on my posts locally, put them on version control and do all other nice stuff we all like to do.

My needs regarding markup are very modest and the aim is to ideally implement a cli client that would allow to edit and publish the posts without web editor like I've done with [cl-journal](https://can3p.github.io/cl-journal/)

There is no public API hence everything in this repo is a result of experiments and reverse engineering, use at your own risk!

## No code solution

First of all, you might not need any api to begin with since substack editor allows you to insert rich content right from the clipboard. Based on my experiments you can add generic html only the editor refuses to paste any data containing substack specific elements like foot notes, it's still an option though.

Here's how I was able to convert markdown from cli and get it directly into clipboard on my linux box:

```
cat substack/drafts/new_post.md| markdown_py | wl-copy -t text/html
```

If you were able to achieve more, let me know!

Substack seems to be using [Prose Mirror](https://prosemirror.net/) for their editor.

## API notes

API root: `https://<blog name>.substack.com/api/v1/`

### Authorization

All requests require a cookie `substack.sid`, which you can get from dev tools.

### Upload a file

Endpoint: POST `<api-root>/image`

Payload: `{ "image": "<base64 encoded image>" }`

Example response: `{"id":"23824782","url":"...","contentType":"image/png","bytes":28315}`

### Posts

#### Create a draft

Endpoint: POST `<api-root>/drafts`

Payload:

```
{
  "draft_title": "",
  "draft_subtitle": "",
  "draft_podcast_url": "",
  "draft_podcast_duration": null,
  "draft_video_upload_id": null,
  "draft_podcast_upload_id": null,
  "draft_podcast_preview_upload_id": null,
  "draft_voiceover_upload_id": null,
  "draft_body": "{\"type\":\"doc\",\"content\":[{\"type\":\"paragraph\",\"content\":[{\"type\":\"text\",\"text\":\"test\"}]}]}",
  "section_chosen": false,
  "draft_section_id": null,
  "draft_bylines": [
    {
      "id": <user id>,
      "is_guest": false
    }
  ],
  "audience": "everyone",
  "type": "newsletter"
}
```

Example response:

```
{
  "type": "newsletter",
  "draft_title": "",
  "draft_subtitle": "",
  "draft_podcast_url": "",
  "audience": "everyone",
  "section_chosen": false,
  "draft_podcast_duration": null,
  "draft_video_upload_id": null,
  "draft_podcast_upload_id": null,
  "draft_podcast_preview_upload_id": null,
  "draft_voiceover_upload_id": null,
  "publication_id": <publication id>,
  "word_count": 1,
  "write_comment_permissions": "everyone",
  "should_send_email": true,
  "show_guest_bios": true,
  "cover_image": null,
  "description": null,
  "search_engine_description": null,
  "search_engine_title": null,
  "slug": null,
  "social_title": null,
  "podcast_description": null,
  "free_unlock_required": false,
  "syndicate_voiceover_to_rss": false,
  "audience_before_archived": null,
  "exempt_from_archive_paywall": false,
  "explicit": null,
  "default_comment_sort": null,
  "draft_body": "{\"type\":\"doc\",\"content\":[{\"type\":\"paragraph\",\"content\":[{\"type\":\"text\",\"text\":\"test\"}]}]}",
  "draft_section_id": null,
  "should_send_free_preview": false,
  "body": null,
  "draft_created_at": "2023-12-20T13:39:52.110Z",
  "draft_updated_at": "2023-12-20T13:39:52.110Z",
  "email_sent_at": null,
  "id": <numeric id>,
  "is_published": false,
  "podcast_duration": null,
  "podcast_url": null,
  "video_upload_id": null,
  "podcast_upload_id": null,
  "podcast_preview_upload_id": null,
  "voiceover_upload_id": null,
  "post_date": null,
  "reply_to_post_id": null,
  "section_id": null,
  "subscriber_set_id": null,
  "subtitle": null,
  "title": null,
  "uuid": "<post uuid>",
  "editor_v2": false,
  "draftVideoUpload": null,
  "draftPodcastUpload": null,
  "podcast_episode_number": null,
  "podcast_season_number": null,
  "podcast_episode_type": null,
  "should_syndicate_to_other_feed": null,
  "syndicate_to_section_id": null,
  "hide_from_feed": null
}
```

The `id` field from the response is needed to update the draft. `draft_body` JSON conforms to [Substack document format](doc_format.md)

#### Update a draft

Endpoint: PUT `<api-root>/drafts/<id>` where `id` is taken from create draft endpoint

Payload: Same as with create draft endpoint

#### Publish a draft

TODO

#### Update a published post

Looks like, you'll need to hit the update draft endpont and then publish the draft all over again, not tested.

## Feedback

If you've like to provide more info, please open a pull request.

## License

All rights for the API belong to substack and the whole repo is just a result of my curiosity.
