# my-rss

A small, static RSS reader that lives in this repository. A scheduled GitHub Action fetches the feeds listed in `feeds.opml`, builds a page, and publishes it. There is no server, no account, and no algorithm deciding what I see.

🔗 **Live:** https://fzero.rubi.gd/my-rss/

## Why

I stopped reading the people I used to read. Not deliberately, they just got buried under infinite scroll, and the places that replaced them are optimised to keep me there rather than to tell me anything. This is the opposite of that: a plain list of sources I picked one at a time, in the order they published, with an end.

The feed list is deliberately **not** seeded from the upstream template's defaults. Those were Hacker News, the GitHub blog and Stack Overflow — firehoses, which is the thing this exists to get away from.

## Credit

Built from **[lovelyRSS](https://github.com/pesarkhobeee/lovelyRSS)** by [Farid Ahmadian](https://github.com/pesarkhobeee), used as a template and MIT licensed. The reader, the build pipeline and the page design are his work. Everything good about how this looks came from there.

## Adding a feed

Edit `feeds.opml`. Each entry needs the feed URL (`xmlUrl`), and it helps to give it a category so it gets its own tab:

```xml
<outline text="Friends" title="Friends">
  <outline text="example" title="example.com" type="rss"
           xmlUrl="https://example.com/feed/"
           htmlUrl="https://example.com/"
           category="Friends"/>
</outline>
```

The site rebuilds on push and on the schedule, so a commit is all that is needed.

Finding the feed URL is usually the annoying part, since most sites no longer advertise it. `/feed/`, `/feed`, `/rss` and `/index.xml` cover most of them. Mastodon profiles work too: append `.rss` to any profile URL.

## Configuration

`config.json` holds the site title, description and item limits. `static/custom.css` overrides the styling. Both are optional; sensible defaults apply without them.

## What it publishes

| File | What it is |
|---|---|
| `index.html` | the reader page |
| `latest_rss.xml` | one merged feed of everything, so someone can follow this whole reading list with a single subscription |
| `latest_feeds.xml` | the source list, most recently updated first |

## Known limits

Worth writing down rather than rediscovering:

- **No history.** The build regenerates from whatever is currently in each source feed. Nothing is stored between runs, so once a publisher rolls an item out of their feed window it is gone from here too. There is no archive and no read/unread state yet.
- **Scheduled workflows are disabled after 60 days without repository activity**, and this applies specifically to public repositories. A reader that quietly stops updating two months after the last commit is the natural failure mode of a set-and-forget repo. Whether a commit made by the Action itself resets that clock is not documented by GitHub and, as far as I can tell, not reliably established anywhere.
- These two are the same problem. Writing fetched items back into the repository would give both an archive and the activity that keeps the schedule alive.

## A note on `.gitignore`

The upstream `.gitignore` excludes `feeds.opml` and `config.json`, because upstream expects forks that pull updates and wants those files to never conflict. This repository was created from the template instead, so there is no upstream to merge from, and the feed list is the most valuable thing here. It is tracked on purpose.

## License

MIT, inherited from [lovelyRSS](https://github.com/pesarkhobeee/lovelyRSS).
