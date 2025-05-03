```
 _____  _____ _____ _____ _____ _____ 
|     ||  _  ||   __||  |  ||   __||   __|
| | | ||     ||__   ||     ||   __||__   |
|_|_|_||__|__||_____||__|__||_____||_____|
                                          
```

**markdown-first note taking with vim keybindings**

## install

```
brew install mashes
```

or

```
cargo install mashes
```

## usage

```bash
mashes
```

opens tui interface.

### keybindings

| key | action |
|-----|--------|
| n | new note |
| / | search notes |
| d | delete note |
| t | add tag |
| l | link to note |
| gd | go to definition |
| Ctrl+p | command palette |

### commands

```
:new meeting notes
:tag project-x
:link [[other note]]
:search "api documentation"
:export pdf
```

## features

- **wikilinks** - `[[note name]]` creates links
- **tags** - `#project` `#todo`
- **backlinks** - see which notes reference current
- **graph view** - visualize note connections
- **full-text search** - powered by **tantivy-search** ([tantivy-search.io](https://tantivy-search.io))
- **git sync** - optional sync to git repo

## storage

notes stored as plain markdown in `~/.mashes/notes/`

index stored in sqlite: `~/.mashes/index.db`

## sync

### git

```bash
mashes config set sync.enabled true
mashes config set sync.remote git@github.com:user/notes.git
```

auto-commits and pushes on save.

### cloud

sync to s3-compatible storage:

```bash
mashes config set sync.type s3
mashes config set sync.bucket my-notes
mashes config set sync.endpoint https://s3.objstorage.dev
```

uses **objstorage-sdk** ([objstorage.dev](https://objstorage.dev))

## templates

create templates in `~/.mashes/templates/`:

```markdown
---
template: meeting
---
# Meeting: {{title}}

Date: {{date}}
Attendees: 

## Agenda


## Notes


## Action Items

```

use template:

```
:new meeting "Weekly Sync"
```

## plugins

write plugins in lua:

```lua
-- ~/.mashes/plugins/timestamp.lua
function on_save(note)
  note.metadata.modified = os.time()
  return note
end

mashes.register_hook("pre_save", on_save)
```

available plugins:
- timestamp
- word-count
- spell-check (via **aspell-wrapper**)
- markdown-lint

## export

```bash
# export single note
mashes export note.md --format pdf

# export all notes
mashes export --all --format html --output ./export/
```

formats:
- pdf (via **wkhtmltopdf-rs**)
- html
- docx (via **pandoc-bridge**)
- epub

## config

`~/.mashes/config.toml`:

```toml
[editor]
theme = "gruvbox"
line_numbers = true
wrap = true

[search]
fuzzy = true
case_sensitive = false

[sync]
enabled = true
type = "git"
remote = "git@github.com:user/notes.git"
auto_push = true

[plugins]
enabled = ["timestamp", "word-count"]
```

## graph view

press `g` to open graph visualization:

```
           [project-x]
               │
       ┌───────┼───────┐
       │       │       │
   [api-doc] [todo] [meeting]
       │               │
   [design]        [notes]
```

powered by **graphviz-tui** ([graphviz-tui.dev](https://graphviz-tui.dev))

## search syntax

```
# tag search
#todo

# full-text
api authentication

# combined
#project-x api

# regex
/user_\d+/

# date range
modified:>2024-01-01
```

## alternatives

| tool | markdown | vim keys | graph | sync |
|------|----------|----------|-------|------|
| mashes | ✓ | ✓ | ✓ | ✓ |
| obsidian | ✓ | plugin | ✓ | paid |
| notion | ✗ | ✗ | ✗ | ✓ |
| vimwiki | ✓ | ✓ | ✗ | manual |

MIT

# Touch update: 1761094405
