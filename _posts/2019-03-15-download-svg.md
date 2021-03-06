---
title: Downloading an SVG File Locally
date: 2019-03-15
layout: post
tags: typescript
description:
thumbnail: /assets/images/svg-logo.svg
---

So! You've created your super awesome SVG, and now you'd like to download that file. The good news is that this is pretty easy to do. Just a few lines attached to a click event.

<svg id="canvas"></svg>
<button id="download" type="button">Download SVG</button>

Here's the SVG details from the graphic shown above.

```html
<svg id="canvas" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="600" height="400">
    <rect x="2" y="2" width="596" height="396" stroke="black" stroke-width="4" fill="none"></rect>
    <rect x="10" y="20" width="300" height="250" fill="darkred"></rect>
    <circle cx="350" cy="250" r="120" fill="darkblue"></circle>
    <text x="350" y="100" fill="green" font-size="24" font-family="'Lucida Sans Unicode', 'Lucida Grande', Helvetica, Arial, sans-serif" font-style="italic">Hello, World!</text>
</svg>
```

First, let's make sure that we have fully defined our SVG by adding the appropriate namespace element. This isn't something that I typically do all the time, but it is necessary for this to work correctly.

```ts
let svg = d3.select<SVGSVGElement, {}>("#canvas").attr("xmlns", "http://www.w3.org/2000/svg");
```

To force the download, we serialize the XML content, create a link, and click the link (from code). The code shown below is written in [TypeScript](https://www.typescriptlang.org/). Figuring out the JavaScript should be trivial.

```ts
d3.select<HTMLButtonElement, {}>("#download").on("click", () => {
    let svg = d3.select<SVGSVGElement, {}>("svg").node() as SVGSVGElement;
    let serializer = new XMLSerializer();
    let source: string = '<?xml version="1.0"?>\n' + serializer.serializeToString(svg);
    let a = document.createElement("a") as HTMLAnchorElement;
    a.download = "image.svg";
    a.href = "data:image/svg+xml;charset=utf-8," + encodeURIComponent(source);
    a.click();
});
```

Once you have downloaded the file, you should be able to open the file in [Inkscape](https://inkscape.org/), or any program that supports editing SVG files.

The complete script is [available in Github](https://github.com/jarrettmeyer/jarrettmeyer.github.io/blob/master/assets/js/download-svg.js).

<script src="https://unpkg.com/d3@5.9.1/dist/d3.min.js"></script>
<script src="/assets/js/download-svg.js"></script>
