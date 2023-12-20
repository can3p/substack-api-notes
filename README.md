# Notes on substack API

Substack is a great platform for writing, they've done a fantastic amount of things in the way others should have years ago. The only thing that's missing for me is a cli client which would allow work on posts locally, put them on version control and do all other nice stuff we all like to do.

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

## Feedback

If you've like to provide more info, please open a pull request.

## License

All rights for the API belong to substack and the whole repo is just a result of my curiosity
