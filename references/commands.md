# Folo CLI Reference

## Global Options

```
--format json|table|plain    Output format, default json
--api-url <url>              API base URL, default https://api.folo.is
--token <token>              Override stored token
--verbose                    Verbose logging
```

## Auth

```
login              Browser OAuth login
logout             Clear session
whoami             Show current user
```

## Timeline

```
timeline --limit 20                       Latest 20 entries
timeline --unread-only --limit 20         Unread only
timeline --feed <feedId> --limit 10       Specific feed
timeline --list <listId> --limit 10       Specific list
timeline --category <name>                By category
timeline --cursor <datetime>              Paginate (use nextCursor)
```

Pagination: response includes `hasNext` + `nextCursor`, loop until `hasNext` is false.

## Entry

```
entry get <entryId>           Full detail (includes content field)
entry read <entryId>          Readability extraction (strips ads/noise)
entry mark-read <entryId>     Mark as read
entry mark-unread <entryId>   Mark as unread
entry mark-all-read           Mark all as read
entry mark-all-read --feed <feedId>
```

## Subscription

```
subscription list                                 List all
subscription add --feed <url> [--category <c>]    Add feed
subscription add --list <listId>                  Add list
subscription remove <id> --target feed|list       Remove
subscription update <id> --title <t> --category <c>  Edit
```

## Search

```
search discover <keyword> [--type feeds|lists]
search rsshub <keyword> [--lang zh]
search trending [--range 1d|3d|7d|30d] [--limit 10] [--language eng|cmn]
```

## Feed

```
feed get <feedId|feedUrl>
feed refresh <feedId>
feed analytics <feedId>
```

## Lists

```
list ls
list get <listId>
list create --title <title> [--description <desc>] [--view <type>]
list update <listId> [--title <title>] [--description <desc>]
list add-feed <listId> --feed <feedId>
list remove-feed <listId> --feed <feedId>
```

## Collections

```
collection list [--limit 20] [--cursor <datetime>]
collection add <entryId>
collection remove <entryId>
```

## OPML

```
opml export --output backup.opml
opml import file.opml [--items url1,url2,...]
```

## Unread

```
unread count
unread list [--view <type>]
```

## Error Recovery

- `UNAUTHORIZED`: run `folo login`
- `HTTP_4xx/5xx`: retry with `--verbose`
- `INVALID_ARGUMENT`: run `<command> --help`

## Environment

- Token stored at: `~/.folo/config.json`
- Login flow: browser OAuth → local callback server → stores token
