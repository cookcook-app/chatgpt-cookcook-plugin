# CookCook for ChatGPT

CookCook is a catalogue of printed cookbooks and the recipes inside them. This
is its ChatGPT plugin: the package submitted to ChatGPT's app directory. It
connects ChatGPT to CookCook's MCP server, so you can ask which cookbook has a
recipe, look inside a book's index, plan a menu from the books you own, keep
your library and grocery list, add the books on a photo of your shelf, and
capture a recipe from a photo of the page.

## What is in this repository

- `plugin.json`: the listing (name, descriptions, starter prompts, logo), the
  review test cases and the release notes.
- `mcp.json`: the server, `https://www.cookcook.app/api/mcp/app`. It works
  signed out for the public catalogue and asks for a CookCook sign-in when a
  library tool is used.
- `skills/get-started/SKILL.md`: which tool answers which question, the shelf
  photo and the recipe photo.
- `skills/menu-planning/SKILL.md`: planning a week of meals or a menu from
  cookbooks.
- `assets/logo.png`: the icon.

## Building the ZIP

The app directory takes a ZIP of this folder's contents, without hidden files:

```bash
rm -f chatgpt-cookcook-plugin.zip && zip -rX chatgpt-cookcook-plugin.zip . -x '.*' '*/.*' 'README.md' '*.zip'
```

Upload it at https://platform.openai.com/plugins. A change to the listing, a
test case or a skill needs a new `version` in `plugin.json`, a new ZIP and a
new review. Tool changes on the server need neither.

- Website: https://www.cookcook.app
- Privacy: https://www.cookcook.app/privacy
- Questions: support@cookcook.app
