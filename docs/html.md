# Code playgrounds in HTML or Markdown pages

Here is how to embed interactive code snippets into your HTML or Markdown pages:

- [Code playgrounds in HTML or Markdown pages](#code-playgrounds-in-html-or-markdown-pages)
  - [Interactive code block](#interactive-code-block)
  - [Attaching to a specific element](#attaching-to-a-specific-element)
  - [Syntax highlighting and rich editing](#syntax-highlighting-and-rich-editing)
  - [Output modes](#output-modes)
  - [Templates](#templates)
  - [Files](#files)
  - [Custom actions](#custom-actions)
  - [Code cells](#code-cells)
  - [Sandbox version](#sandbox-version)
  - [Global settings](#global-settings)
    - [Self-hosted server](#self-hosted-server)
    - [Run timeout](#run-timeout)

## Interactive code block

Let's start with a simple use case. Suppose you have a static code snippet in Python:

```html
<pre><code>
msg = "Hello, World!"
print(msg)
</code></pre>
```

To make it interactive, add a `codapi-snippet` element right after the `pre` element:

```html
<pre><code>
msg = "Hello, World!"
print(msg)
</code></pre>

<codapi-snippet sandbox="python" editor="basic"></codapi-snippet>
```

Note two properties here:

-   `sandbox` defines the engine that will execute the code. Usually it's the name of the language or software, like `python`, `bash` or `sqlite`.
-   `editor` = `basic` enables code snippet editing.

Finally, include the default styles in the `head`:

```html
<link rel="stylesheet" href="https://unpkg.com/@antonz/codapi@0.19.7/dist/snippet.css" />
```

And the JavaScript file at the bottom of the page:

```html
<script src="https://unpkg.com/@antonz/codapi@0.19.7/dist/snippet.js"></script>
```

(CDNs like unpkg can sometimes be slow, so it's even better to host both files yourself)

And that's it! The `codapi-snippet` will automatically attach itself to the `pre`, allowing you to run and edit the code:

```
┌───────────────────────────────┐
│ msg = "Hello, World!"         │
│ print(msg)                    │
│                               │
│                               │
└───────────────────────────────┘
  Run  Edit
```

To disable editing, set `editor` = `off` instead of `editor` = `basic`. To change the engine, set the appropriate `sandbox` value.

## Attaching to a specific element

To attach `codapi-snippet` to the specific code element (instead of using the preceding element), set the `selector` property to its CSS selector:

```html
<div id="playground">
    <pre class="code">
msg = "Hello, World!"
print(msg)
    </pre>
</div>

<!-- more HTML -->

<codapi-snippet sandbox="python" editor="basic" selector="#playground .code">
</codapi-snippet>
```

## Syntax highlighting and rich editing

To use `codapi-snippet` with code editors like CodeMirror, do the following:

1. Initialize the editor as usual.
2. Point `codapi-snippet` to the editor using the `selector` property.
3. Set `editor="external"` so that `codapi-snippet` does not interfere with the editor functions.

```html
<div id="editor"></div>

<!-- ... -->

<codapi-snippet sandbox="python" editor="external" selector="#editor">
</codapi-snippet>
```

## Output modes

The widget supports multiple [output modes](output-modes.md), from plain text to SVG images, HTML fragments, and interactive DOM.

## Templates

[Templates](templates.md) help to keep snippets concise by hiding parts of the code behind the scenes.

## Files

For larger programs, you can pass multiple [files](files.md) along with the main one.

## Custom actions

You can add buttons to the toolbar to [run commands and trigger events](custom-actions.md).

## Code cells

[Code cells](code-cells.md) are snippets that depend on each other. This results in Jupyter notebook-like behavior and eliminates the need for code duplication.

## Sandbox version

Some sandboxes support multiple versions (e.g. the latest released version and the next beta or release candidate version). By default, the widget uses the `latest` version, but you can change this using the `version` attribute:

```html
<codapi-snippet sandbox="sqlite:dev" editor="basic">
</codapi-snippet>
```

The appropriate version must be enabled on the server.

## Global settings

To change the Codapi settings, use the `codapi-settings` element. First, load it from the CDN:

```html
<script src="https://unpkg.com/@antonz/codapi@0.19.7/dist/settings.js"></script>
```

Then use it like this:

```html
<codapi-settings prop1="value1" prop2="value2"... propN="valueN">
</codapi-settings>
```

Or import the `codapi` object and set the settings in the code:

```js
import { codobj } '@antonz/codapi/dist/settings.mjs';

codobj.settings.prop1 = "value1";
codobj.settings.prop2 = "value2";
// ...
codobj.settings.propN = "valueN";
```

See the list of available settings below.

### Self-hosted server

If you are using a self-hosted [codapi server](https://github.com/nalgeon/codapi), point the widget to it using the `url` setting:

```html
<codapi-settings url="http://localhost:1313/v1">
</codapi-settings>
```

Specify the protocol (`http`/`https`) and hostname or IP like this:

```
http://localhost:1313/v1
http://192.168.1.42:1313/v1
https://codapi.example.org/v1
```

Note that the `/v1` part stays the same — it's the path the Codapi server uses for API requests.

### Run timeout

The default run timeout is 10000ms (10s). If you are expecting your snippet to run for longer than that you can specify a diffent timeout using the `timeout` setting:

```html
<codapi-settings timeout="15000">
</codapi-settings>
```
