# Bluesky Feed JSON for Skyfeed

This folder contains feed definitions for Bluesky feeds built in Skyfeed.

Each `.json` file is a Skyfeed configuration. You can keep these files as templates, duplicate them, and adjust the filters to build different custom feeds.

## Files in this folder

- `ontario canada politics.json`: a feed focused on Ontario and Canada politics content
- `funny canadian.json`: a feed focused on funny Canadian content, slang, memes, and related posts

## How to use these JSON files in Skyfeed

1. Open Skyfeed.
2. Create a new feed or open an existing one.
3. Recreate the blocks from the JSON file in Skyfeed, or paste/import the JSON if your workflow supports that.
4. Test the feed results.
5. Adjust keywords, regex rules, tags, and exclude filters until the feed quality looks right.

## Basic JSON structure

Each feed file follows the same overall pattern:

```json
{
	"displayName": "Feed Name",
	"blocks": [
		{
			"type": "input",
			"inputType": "firehose",
			"firehoseSeconds": 259200
		},
		{
			"type": "regex",
			"value": "(?i)\\b(keyword1|keyword2)\\b",
			"target": "text|alt_text|link"
		},
		{
			"type": "sort",
			"sortType": "created_at"
		}
	],
	"license": "EUPL-1.2"
}
```

## What the block types do

### `input`

Defines where posts come from.

Common examples:

- `firehose`: scans all recent Bluesky posts
- `tags`: limits feed input to posts with specific hashtags

Example:

```json
{
	"type": "input",
	"inputType": "firehose",
	"firehoseSeconds": 259200
}
```

### `remove`

Filters out unwanted content patterns.

Common uses:

- remove reposts
- remove hellthreads

Example:

```json
{
	"type": "remove",
	"subject": "item",
	"value": "repost"
}
```

### `regex`

Matches or excludes posts based on text patterns.

Common fields:

- `value`: the regex pattern
- `target`: where to search, often `text|alt_text|link`
- `invert: true`: reverses the match, so matching posts are excluded instead of included

Example include filter:

```json
{
	"type": "regex",
	"value": "(?i)\\b(canada|canadian|canuck)\\b",
	"caseSensitive": false,
	"target": "text|alt_text|link"
}
```

Example exclude filter:

```json
{
	"type": "regex",
	"value": "(?i)\\b(news|politics|crime)\\b",
	"caseSensitive": false,
	"invert": true
}
```

### `sort`

Controls feed ordering.

Example:

```json
{
	"type": "sort",
	"sortType": "created_at"
}
```

## How to customize a feed

The most effective way to build a good feed is usually:

1. Start with a broad input source.
2. Add 1 to 3 strong include filters.
3. Add a few targeted exclude filters.
4. Sort by recency.
5. Test and refine.

### Good include ideas

- location words
- community slang
- hashtags
- topic-specific keywords
- combinations like identity plus humor, or region plus politics

### Good exclude ideas

- reposts
- hellthreads
- unrelated countries
- serious news terms
- spammy phrases

## Regex tips for Skyfeed

### Use word boundaries

This reduces accidental matches:

```regex
\b(canada|canadian|canuck)\b
```

### Use case-insensitive matching

This catches upper- and lower-case variants:

```regex
(?i)\b(funny|meme|comedy)\b
```

### Try reverse-order duplicates when results differ

In some feed setups, it can help to duplicate a regex block with the same terms in reverse order.

Example:

```json
{
  "id": "include01",
  "type": "regex",
  "value": "(?i)\\b(canada|canadian|canuck|eh)\\b",
  "caseSensitive": false,
  "target": "text|alt_text|link"
},
{
  "id": "include02",
  "type": "regex",
  "value": "(?i)\\b(eh|canuck|canadian|canada)\\b",
  "caseSensitive": false,
  "target": "text|alt_text|link"
}
```

This is worth testing when Skyfeed seems sensitive to regex ordering.

## Example feed recipes

### Politics feed

Recipe:

- firehose input
- location keywords
- politics keywords
- exclude unrelated countries or topics
- sort by recent posts

### Humor feed

Recipe:

- firehose or tag input
- culture/slang keywords
- humor keywords
- exclude politics, crime, and heavy news
- sort by recent posts

## Workflow for making a new feed

1. Copy an existing JSON file.
2. Rename `displayName`.
3. Update tags and regex terms.
4. Keep the `remove` blocks unless you have a reason not to.
5. Test the feed in Skyfeed.
6. Tighten include and exclude rules based on bad matches.

## Suggested naming approach

Use descriptive names so you can tell feeds apart quickly.

Examples:

- `canadian tech.json`
- `ontario housing.json`
- `canadian sports memes.json`
- `maritime news.json`

## Notes

- `firehoseSeconds: 259200` is 3 days of posts.
- `text|alt_text|link` is useful when you want to match post text, image alt text, and linked URLs together.
- If tags bring in too much unrelated content, remove them and rely more on regex filters.
- If the feed is too narrow, broaden the include terms before reducing exclude rules.

## License

These files currently use `EUPL-1.2` in the JSON metadata.
