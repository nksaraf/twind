# Installation

It is possible to install and thus use Twind in a multitude of different ways. We expose various different modules – from the latest es syntax to umd builds – with the aim of accommodating for as many dev setups as possible. This said, for the smallest size and fastest performance we recommend you use the module build.

> Although compatible with traditional bundlers no build step is required to use the module

<details><summary>Table Of Contents (Click To Expand)</summary>

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Importing as a local dependency](#importing-as-a-local-dependency)
- [Importing as a remote dependency](#importing-as-a-remote-dependency)
- [twind/shim](#twindshim)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
</details>

## Importing as a local dependency

Most build tools rely on modules to be installed locally on the machine they are running on. Usually these modules are available on and installed via npm. Twind is no different in this regard.

1. Run the following command in your terminal, from your project root:

```sh
npm i twind
```

2. Then go ahead and import the module into your application using the bare module specifier:

```js
import { tw, setup } from 'twind'
```

Assuming you have your bundler configured correctly then you should now be able to just use the module.

## Importing as a remote dependency

Given that nearly all [browsers support es modules](https://caniuse.com/es6-module) now, sometimes it is desirable to import a module straight from from a CDN such as [skypack](https://skypack.dev/) or [unpkg](https://unpkg.com/).

1. Add the following line to a javascript file referenced by a script tag with `type="module"` like below:

```html
<script type="module">
  import { tw, setup } from 'https://cdn.skypack.dev/twind'
</script>
```

Assuming you have an internet connection then you should now be able to use the module.

> [live and interactive demo](https://esm.codes/#aW1wb3J0IHsgdHcgfSBmcm9tICdodHRwczovL2Nkbi5za3lwYWNrLmRldi90d2luZCcKCmRvY3VtZW50LmJvZHkuaW5uZXJIVE1MID0gYAogIDxtYWluIGNsYXNzPSIke3R3YGgtc2NyZWVuIGJnLXB1cnBsZS00MDAgZmxleCBpdGVtcy1jZW50ZXIganVzdGlmeS1jZW50ZXJgfSI+CiAgICA8aDEgY2xhc3M9IiR7dHdgZm9udC1ib2xkIHRleHQoY2VudGVyIDV4bCB3aGl0ZSBzbTpncmF5LTgwMCBtZDpwaW5rLTcwMClgfSI+CiAgICAgIFRoaXMgaXMgVHdpbmQhCiAgICA8L2gxPgogIDwvbWFpbj4KYA==)

<details><summary>How to support legacy browser with the UMD bundles (Click to expand)</summary>

> You may need to provide certain [polyfills](./browser-support.md) depending on your target browser.

```html
<script src="https://unpkg.com/twind/twind.umd.js"></script>
<script>
  var tw = twind.tw
  var setup = twind.setup
</script>
```

</details>

## twind/shim

> Allows to copy-paste tailwind examples. This feature can be used together with your favorite framework without any additional setup.

The `twind/shim` modules allows to use the `class` attribute for tailwind rules. If such a rule is detected the corresponding CSS rule is created and injected into the stylesheet. _No need for `tw`_ but it can be used on the same page as well (see example below).

```html
<!DOCTYPE html>
<html lang="en" hidden>
  <head>
    <script type="module" src="https://cdn.skypack.dev/twind/shim"></script>
  </head>
  <body>
    <main class="h-screen bg-purple-400 flex items-center justify-center">
      <h1 class="font-bold text(center 5xl white sm:gray-800 md:pink-700)">This is Twind!</h1>
    </main>
  </body>
</html>
```

> [live and interactive shim demo](https://esm.codes/#aW1wb3J0ICdodHRwczovL2Nkbi5za3lwYWNrLmRldi90d2luZC9zaGltJwoKZG9jdW1lbnQuYm9keS5pbm5lckhUTUwgPSBgCiAgPG1haW4gY2xhc3M9Imgtc2NyZWVuIGJnLXB1cnBsZS00MDAgZmxleCBpdGVtcy1jZW50ZXIganVzdGlmeS1jZW50ZXIiPgogICAgPGgxIGNsYXNzPSJmb250LWJvbGQgdGV4dChjZW50ZXIgNXhsIHdoaXRlIHNtOmdyYXktODAwIG1kOnBpbmstNzAwKSI+CiAgICAgIFRoaXMgaXMgVHdpbmQhCiAgICA8L2gxPgogIDwvbWFpbj4KYA==)

All twind syntax features like [grouping](./grouping.md) are supported. See [example/shim.html](https://github.com/tw-in-js/twind/blob/main/example/shim.html) for a full example.

To customize the default `tw` instance you can provide a `<script type="twind-config">...</script>` within the document. The content must be valid JSON and all [twind setup options](./setup.md) (including [hash](https://github.com/tw-in-js/twind/blob/main/docs/setup.md#hash)) are supported.

```html
<!DOCTYPE html>
<html lang="en" hidden>
  <head>
    <script type="module" src="https://cdn.skypack.dev/twind/shim"></script>
    <script type="twind-config">
      {
        "hash": true
      }
    </script>
  </head>
  <body>
    <h1 class="text-7xl rounded-md ring(& pink-700 offset(4 pink-200))">Hello World</h1>
  </body>
</html>
```

Alternatively the following works:

```js
import { setup } from "https://cdn.skypack.dev/twind/shim"

setup({
  target: document.body, // Default document.documentElement (eg html)
  ... // All other twind setup options are supported
})
```

It is possible to mix `twind/shim` with `tw`:

```js
import 'twind/shim'
import { tw } from 'twind'

const styles = {
  center: tw`flex items-center justify-center`,
}

document.body.innerHTML = `
  <main class="h-screen bg-purple-400 ${styles.center}">
    <h1 class="font-bold ${tw`text(center 5xl white sm:gray-800 md:pink-700)`}">
      This is Twind!
    </h1>
  </main>
`
```

To prevent FOUC (flash of unstyled content) it is advised to set the `hidden` attribute on the target element. `twind/shim` will remove it once all styles have been generated.

```html
<!DOCTYPE html>
<html lang="en" hidden>
  <!-- ... -->
</html>
```

<details><summary>How can I use twind/shim from javascript (Click to expand)</summary>

> Internally `twind/shim` uses [twind/observe](./observe.md) which may be useful for advanced use cases.

```js
import 'twind/shim'
```

```js
import { setup, disconnect } from 'twind/shim'
```

</details>

<details><summary>Static Extraction a.k.a. Server Side Rendering (Click to expand)</summary>

**Synchronous SSR**

```js
import { setup } from 'twind'
import { virtualSheet, getStyleTag, shim } from 'twind/shim/server'

const sheet = virtualSheet()

setup({ ...sharedOptions, sheet })

function ssr() {
  // 1. Reset the sheet for a new rendering
  sheet.reset()

  // 2. Render the app to an html string and handle class attributes
  const body = shim(renderTheApp())

  // 3. Create the style tag with all generated CSS rules
  const styleTag = getStyleTag(sheet)

  // 4. Generate the response html
  return `<!DOCTYPE html>
    <html lang="en">
      <head>${styleTag}</head>
      <body>${body}</body>
    </html>
  `
}
```

**Asynchronous SSR**: see [WMR example in SSR docs](./ssr.md#wmr)

**Custom `tw` instances**

`shim` accepts an optional second argument which should be the `tw` instance to use.

```js
import { create } from 'twind'

const sheet = virtualSheet()

const { tw } create({ ...sharedOptions, sheet })

function ssr() {
  // Same as before

  // 2. Render the app to an html string and handle class attributes
  const body = shim(renderTheApp(), tw)

  // Same as before
}
```

</details>

<details><summary>How to support legacy browser with the UMD bundles (Click to expand)</summary>

> You may need to provide certain [polyfills](./browser-support.md) depending on your target browser.

```html
<script defer src="https://unpkg.com/twind/twind.umd.js"></script>
<script defer src="https://unpkg.com/twind/observe/observe.umd.js"></script>
<script defer src="https://unpkg.com/twind/shim/shim.umd.js"></script>
```

</details>

<details><summary>Implementation Details (Click to expand)</summary>

`twind/shim` starts [observing](./observe.md) class attributes changes right after the [DOM content has been loaded](https://developer.mozilla.org/en-US/docs/Web/API/Document/DOMContentLoaded_event). For further details see [twind/observe - Implementation Details](./observe.md#implementation-details).

</details>

<hr/>

Continue to [Setup](./setup.md)
