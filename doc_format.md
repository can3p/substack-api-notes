# Substack document format

Substack document is sent in a field `draft_body`, basic shape of the json is following:

```
{
  "type": "doc",
  "content": [
    {
      "type": "<block type>",
      "attrs": { ... },
      "content": [ <inline nodes> ],
    },
  ]
}
```

Where `content` fields are arrays containing a list of nodes in the element. Top level content is a list of block nodes, nested content is a list of inline nodes

## Top level nodes

### Heading

```
{
    "type": "heading",
    "attrs": {
        "level": <1-6>
    },
    "content": [ <inline text nodes> ]
}
```

### Paragraph

```
{
    "type": "paragraph",
    "content": [ <inline text nodes> ]
}
```

### Captioned image

```
{
    "type": "captionedImage",
    "content": [
    {
        "type": "image2",
        "attrs": {
            "src": "<image url>",
            "srcNoWatermark": null,
            "fullscreen": null,
            "imageSize": null,
            "height": 360,
            "width": 413,
            "resizeWidth": null,
            "bytes": 28315,
            "alt": null,
            "title": null,
            "type": "image/png",
            "href": null,
            "belowTheFold": false,
            "topImage": false,
            "internalRedirect": null
        }
    },
    {
        "type": "caption",
        "content": [ <inline text nodes> ]
    }
    ]
}
```

`caption` is yet another text container and hence accepts an array of inline text nodes.

### Image gallery

```
{
    "type": "imageGallery",
    "attrs": {
    "gallery": {
        "images": [
        {
            "type": "image/jpeg",
            "src": "<url>"
        },
        {
            "type": "image/jpeg",
            "src": "<url>"
        }
        ],
        "caption": "",
        "alt": "",
        "staticGalleryImage": {
            "type": "image/png",
            "src": "<url>"
        }
    },
    "isEditorNode": true,
    "isEditor": true
    }
},
```

### Footnote

```
{
    "type": "footnote",
    "attrs": {
        "number": 1
    },
    "content": [ <text nodes> ]
}
```

Apparently a footnote can contain an array of inline nodes as well as an array of block nodes, limitation have not been tested yet.

## Inline text nodes

While specific block elements like captioned image have specific requirements for nested elements, others like headings or paragraph simply allow to have formatted text inline. The text is represented as an array of elements. Here is a basic example:

```
{
    "type": "text",
    "text": "i"
},
{
    "type": "text",
    "marks": [
        {
            "type": "strong"
        }
    ],
    "text": "ntr"
},
{
    "type": "text",
    "text": "o"
}
{
    "type": "footnoteAnchor",
    "attrs": {
        "number": 1
    }
}
```

`marks` field allows to format the text, `footnoteAnchor` is a special inline element that adds a foot note. I think it's up to the editor to assign numbers correctly.
