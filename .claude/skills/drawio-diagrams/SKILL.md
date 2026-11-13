---
name: drawio-diagrams
description: Draw every diagram of a CodeJourney post with draw.io as an editable `*.drawio.svg` file, never with Mermaid. Use when a post needs a diagram, chart of boxes and arrows, architecture picture, flow, sequence or state diagram, or when a post contains a Mermaid code block.
---

# Diagrams with draw.io

Draw every diagram in a post with draw.io. Do not use Mermaid, PlantUML, or ASCII art for diagrams.

## Why

- A `*.drawio.svg` file is a normal SVG image, so the browser shows it without a plugin or a script.
- The same file also holds the draw.io source of the diagram. Anyone can open the file in draw.io, change it, and save it. No export step is needed.
- Mermaid needs a script in the browser, and its layout changes between versions.

## File name and location

- Save each diagram as `public/images/diagrams/<post-slug>-<name>.drawio.svg`, for example `public/images/diagrams/kubernetes-for-solo-developers-traffic.drawio.svg`.
- Use lowercase words and hyphens in `<name>`.
- Always use the double extension `.drawio.svg`. The draw.io editors use that extension to open the file as an editable diagram.

## Use the diagram in a post

Use a normal Markdown image with a path from the site root:

```md
![How a request reaches the pod: browser, Cloudflare, ingress, service, pod](/images/diagrams/kubernetes-for-solo-developers-traffic.drawio.svg)
```

- Write alt text that says what the diagram shows, not only its title. Screen readers and search engines read the alt text.
- Do not paste a Mermaid code block (` ```mermaid `) into a post. When you find one, replace it with a draw.io diagram.

## Create or edit a diagram

### In VS Code (preferred)

1. Install the extension "Draw.io Integration" (`hediet.vscode-drawio`).
2. Create an empty file with the name from above, for example `public/images/diagrams/my-post-flow.drawio.svg`.
3. Open the file. VS Code shows the draw.io editor.
4. Draw the diagram and save. The extension writes the SVG image and the draw.io source into the same file.

### In the browser or the desktop app

1. Open https://app.diagrams.net or the draw.io desktop app.
2. Open the existing `.drawio.svg` file, or create a new diagram.
3. Choose **File > Save as**, then the format **Editable Vector Image (.svg)**. For a new diagram, type the full name ending in `.drawio.svg`.

### As an AI agent

An agent cannot click in the draw.io editor. Write the file directly, then ask the owner to open it once in draw.io and save it.

- The root `<svg>` element needs the attribute `content`. That attribute holds the draw.io source: an `<mxfile>` element with one `<diagram>` and an uncompressed `<mxGraphModel>`. Escape the XML for an attribute (`&lt;`, `&gt;`, `&quot;`, `&amp;`).
- The rest of the `<svg>` element draws the same boxes, lines and text, so the image shows correctly before anyone opens it in draw.io.
- Keep the positions and sizes of the shapes the same in the `mxGraphModel` and in the SVG drawing.
- When the draw.io desktop app is installed, let the app draw the SVG instead. Write the source to a temporary `.drawio` file, then run:

  ```sh
  drawio --export --format svg --embed-diagram --output public/images/diagrams/<file>.drawio.svg <temp>.drawio
  ```

A minimal file with one box looks like this:

```xml
<svg xmlns="http://www.w3.org/2000/svg" width="162" height="62" viewBox="-0.5 -0.5 162 62" content="&lt;mxfile&gt;&lt;diagram id=&quot;d1&quot; name=&quot;Page-1&quot;&gt;&lt;mxGraphModel&gt;&lt;root&gt;&lt;mxCell id=&quot;0&quot;/&gt;&lt;mxCell id=&quot;1&quot; parent=&quot;0&quot;/&gt;&lt;mxCell id=&quot;2&quot; value=&quot;Ingress&quot; style=&quot;rounded=1;whiteSpace=wrap;html=1;&quot; vertex=&quot;1&quot; parent=&quot;1&quot;&gt;&lt;mxGeometry x=&quot;0&quot; y=&quot;0&quot; width=&quot;160&quot; height=&quot;60&quot; as=&quot;geometry&quot;/&gt;&lt;/mxCell&gt;&lt;/root&gt;&lt;/mxGraphModel&gt;&lt;/diagram&gt;&lt;/mxfile&gt;">
  <rect x="0" y="0" width="160" height="60" rx="9" fill="#ffffff" stroke="#000000"/>
  <text x="80" y="35" text-anchor="middle" font-family="Helvetica" font-size="12">Ingress</text>
</svg>
```

## Style

- Make the diagram readable in light mode and in dark mode. Give the diagram a white background (draw.io: **File > Properties > Background**), or use colors that have enough contrast on both.
- Use the font Helvetica or the draw.io default font. Do not use a font that the reader may not have installed.
- Keep the text at 12 px or larger.
- Keep a diagram to one idea. Split a large diagram into several smaller ones.

## Before you commit

- Open the post in the local site (`pnpm dev` in `hayone1/codejourney`) and check that the diagram shows. The site copies `public/` of this repository when the dev server starts, so restart the dev server after you add or change a diagram.
- The site does not serve top-level files of `public/`, folders that start with `_` or `.`, the folder `api`, or HTML files. Keep diagrams in `public/images/diagrams/`.
- Check that the file opens in draw.io as an editable diagram, not as a flat picture.
