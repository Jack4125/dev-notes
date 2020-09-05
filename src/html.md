---
title: "HTML"
tags: "HTML 5"
---

# HTML

[Favicon Generator](https://favicon.io/favicon-generator/)

## HTML Basic

- `<q>` and `<blockquote>` for quoting.

- `<abbr>` for abbreviations.  
  Example: `<p>The <abbr title="World Health Organization">WHO</abbr></p>`

- `<form action="" method="">` - do not forget to include "method".

- Entities:

  - `&nbsp;` - Non-Breaking Space  
    _Prevent two words from being broken up or wrapped to new line_ when page width changes.
  - `&copy;` - Copyright Symbol

- `<svg>` vs `<canvas>`:

  ![SVG vs Canvas](../../images/svg-canvas.PNG)

## HTML 5

- `<picture>` - dynamic image rendering depending on screen size.

- `<figure>` and `<figcaption>` add description to images.

- Form:

  - `<input type="">` accepts special types like `reset`, `file`, `search`, `tel`, `number`.

  - `<datalist>` - Works like `<select>` + `<options>`, but accept any input value.

  - `<output name="">` - updates whenever name.value changes

  - Multiple submit buttons for different parts of a form:

    - `<input type="submit" formaction="">` - overrides the action url of `<form action="">`

    - `<input type="submit" formmethod="">` - overrides the method of `<form method="">`

- `<embed>` is used to embed external content such as another html page or canvas.

  > Note: user `<video>` and `<audio>` for video and audio.
  > Or use `<iframe>` to embed YouTube Videos. Go to the YouTube video you want to use, click `share`, click `embed`

- `HTML5 Shiv` provides browser support for semantic tags like `<main>` or `<article>` before _IE9_:

  ```html
  <head>
    <!--[if lt IE 9]>
      <script src="/js/html5shiv.js"></script>
    <![endif]-->
  </head>
  ```

### APIs

- Web Storage:

  - `localStorage` object stores data per origin (domain and protocol).

  - `sessionStorage` object stores data until browser tab is closed.

- Web Worker:

  - Uses `Worker` constructor.

  - A script that runs on a separate thread, so it's non-blocking of main script.

  - Located in external files, so do not have `window`, `document`, or `parent` objects.

- Geolocation:

  - `navigator` object has a `geolocation` API with multiple methods:

  - `getCurrentPosition()` always returns:

    - position.coords.latitude

    - position.coords.longitude

    - position.coords.accuracy

- Server-Sent Events

  - Communicate uni-directional from server to client without sending requests (event driven).

  - `EventSource(url)` object receives server-sent data.

  - `onmessage` event processes `event.data`

  - For bi-directional communication, see [WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API).

- HTML Element:

  - [Dataset](https://github.com/riot/riot/issues/1001):

    - Attaches some user defined data to the html element.

    - Can be used to pass data along with event (since event has to be the only parameter passed).

    - HTLM: `<div data-thing-to-attach="some data">`

    - JS: `(div element).dataset.thingToAttach === "some data"`

* Drag and Drop:
  - Part of HTML5 standard. Can be used on any element.
  - Including attribute `draggable="true"` in a tag.
  - Events: `ondragstart`, `ondragover`, `ondrop`
  - Methods: `dataTransfer.setData()`

### [Accessibility](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/WAI-ARIA_basics#Practical_WAI-ARIA_implementations)

- `role="navigation"`

- `aria-label="Search through site content"`

- `aria-live="assertive" aria-atomic="true"`
