# Social Network

[social.jake.fun](https://social.jake.fun)

Publish three files, and share yourself with the world.

Someone should build a reader...

...check [social.jake.fun/feed](https://social.jake.fun/feed) for a basic example.

## Instructions

Participation is publishing two JSON files to the internet.
The first is your `profile.json` which is an object describing yourself.
The second is your `stream.json` which is an array of your posts.

Anyone that knows about your profile and stream can access it on the internet.
If someone knows about many streams, they can collect them all together in a "feed" to be displayed however they like.

### What does the data look like?

Your profile and stream can look _however_ you want them to look. As long as they are valid JSON, they can be read by anyone.

However, if you are looking to do this with other people, you'll want the structure of your JSON to follow at least _some_ rules so that a reader knows how to handle your stream and profile.

### profile.json

Currently, I am proposing the following for a minimal `profile.json`

```json
{
  "@": "jandoe",
  "name": "Jan Doe",
  "pronouns": "whoever/you/are",
  "location": "Anytown, USA",
  "bio": "normal bio [with links](https://links.com)",
  "website": "https://jandoe.com",
  "image": {
    "url": "https://website.com/image.jpg",
    "alt": "A picture of me looking directly into the camera",
    "height": 600,
    "width": 600
  }
}
```

### stream.json

For `stream.json` the only important value is a `time` value. This way posts can be ordered when streams are combined into a feed. For that, use a millisecond epoch. You can get that by running `Date.now()` in a JavaScript console, or using a tool like [currentmillis.com](https://currentmillis.com/). The number should be 13 digits long and in the JSON as numbers (not strings).

Currently, I am proposing something like this for a basic stream that supports text, image, and link posts:

```json
[
  {
    "text": "a text post with an image",
    "image": {
      "url": "https://image.com/image.jpg",
      "alt": "alt text of the image.",
      "height": 600,
      "width": 600
    },
    "time": 1650938694864
  },
  {
    "text": "a text post with a link at the end",
    "url": "https://link.com/something",
    "time": 1650938170000
  },
  {
    "text": "a pure text post",
    "time": 1650938164497
  }
]
```

These can really look however you want them to though. No one is saying you have to do it this way. I am making a reader that reads ^ this format, but you can make a reader that reads whatever you and your friends want to post.

### Threads / Replies

This is very much WIP, but here's a proposal for threads and replies.

It is important that you host your stream at a location that wont change, that way your friends can easily find you and communicate with you.

Your stream has one message and it looks like this, hosted at `social.yourwebsite.com`:

```json
[
  {
    "text": "a pure text post",
    "time": 1650938164497
  }
]
```

You could thread to that status using the following syntax:

```json
[
  {
    "text": "@social.yourwebsite.com#1650938164497 this one is a reply",
    "time": 1650938694864
  },
  {
    "text": "a pure text post",
    "time": 1650938164497
  }
]
```

The format is `@ + domain + # + time`. This way a reader can find the post an link to it, visualize the thread however it wants. You can use this to reply to another stream's thread as well, since the domain is the unique identifier for each stream.

```json
[
  {
    "text": "@stream.anotherwebsite.com#1650933416323 hiya, pal!",
    "time": 1650938695555
  },
  {
    "text": "@social.yourwebsite.com#1650938164497 this one is a reply",
    "time": 1650938694864
  },
  {
    "text": "a pure text post",
    "time": 1650938164497
  }
]
```

## Readers

A reader will only read the streams it is subscribed to following the formats it expects. That means if you want to read your own streams, you'll have to make one until someone makes a tool that makes this easier. I have a working example in this repo at [feed/index.html](feed/index.html) that is following a few streams. I also created an unstyled example on [CodePen](https://codepen.io/jakealbaugh/pen/abEgGQd/f7c9f5c6d2c5ac7d0ef29b433b6a2e0c) if you want to do it that way.
