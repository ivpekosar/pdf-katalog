# Flipbook Viewer

<div class="overflow-x-auto w-full px-2 mb-6"><table class="min-w-full border-collapse text-sm leading-[1.7] whitespace-normal"><thead class="text-left"><tr><th scope="col" class="text-text-100 border-b-0.5 border-border-300/60 py-2 pr-4 align-top font-bold">Co</th><th scope="col" class="text-text-100 border-b-0.5 border-border-300/60 py-2 pr-4 align-top font-bold">URL</th></tr></thead><tbody><tr><td class="border-b-0.5 border-border-300/30 py-2 pr-4 align-top">Otevřít katalog</td><td class="border-b-0.5 border-border-300/30 py-2 pr-4 align-top"><code class="bg-text-200/5 border border-0.5 border-border-300 text-danger-000 whitespace-pre-wrap rounded-[0.4rem] px-1 py-px text-[0.9rem]">?file=Katalog_obklady_Kospan_2026_01_CZ_lamely</code></td></tr><tr><td class="border-b-0.5 border-border-300/30 py-2 pr-4 align-top">Konkrétní strana</td><td class="border-b-0.5 border-border-300/30 py-2 pr-4 align-top"><code class="bg-text-200/5 border border-0.5 border-border-300 text-danger-000 whitespace-pre-wrap rounded-[0.4rem] px-1 py-px text-[0.9rem]">?file=Katalog_obklady_Kospan_2026_01_CZ_lamely&amp;page=15</code></td></tr><tr><td class="border-b-0.5 border-border-300/30 py-2 pr-4 align-top">Další katalog</td><td class="border-b-0.5 border-border-300/30 py-2 pr-4 align-top"><code class="bg-text-200/5 border border-0.5 border-border-300 text-danger-000 whitespace-pre-wrap rounded-[0.4rem] px-1 py-px text-[0.9rem]">?file=jiny-katalog</code></td></tr></tbody></table></div>

https://ivpekosar.github.io/pdf-katalog/?file=Katalog_obklady_Kospan_2026_01_CZ_lamely


## Advantages

1. Tiny (18 ***Kb***). For comparison, the amazing [page-flip](./https://www.npmjs.com/package/page-flip) is 10 **Mb** (x1000 times bigger!).
2. Can use any input as a book simply by plugging in a “book provider”. An example PDF book using the amazing [pdfjs](./https://www.npmjs.com/package/pdfjs-dist) from Mozilla can be found in the test folder—[book-pdf.js](./test/book-pdf.js) (referenced usage: [test-pdf.js](./test/test-pdf.js))
3. Supports **Panning**, **Zooming**, **Liking**, **Sharing**, along with page turning effects.
4. Raises events to track which pages are being viewed by user.

## Usage

Below shows the flip `book` on the given `div` with the id `div-id`:

```js
'use strict'

import { init as flipbook } from 'flipbook-viewer';

...

flipbook(book, 'div-id', (err, viewer) => {
  if(err) console.error(err);

  console.log('Number of pages: ' + viewer.page_count);
  viewer.on('seen', n => console.log('page number: ' + n));

  next.onclick = () => viewer.flip_forward();
  prev.onclick = () => viewer.flip_back();
  zoom.onclick = () => viewer.zoom();

});
```

The viewer can show *any* flip book. All you need to do is provide a book interface:

```js
{
  numPages: () => {
    /* return number of pages */
  },
  getPage: (num, cb) => {
    /* return page number 'num'
     * in the callback 'cb'
     * as any CanvasImageSource:
     * (CSSImageValue, HTMLImageElement, 
     *  SVGImageElement, HTMLVideoElement,
     *  HTMLCanvasElement, ImageBitmap,
     *  OffscreenCanvas)
     */
  }
}
```

## Options

An optional `opts` parameter can be passed in to change the UI:

```js
const opts = {
  backgroundColor: "#353535",
  boxColor: "#353535",
  width: 800,
  height: 600,
}

flipbook(book, 'div-id', opts, (err, viewer) => ...
```

## Events

You can listen on the `viewer` for which pages were seen:

```js
viewer.on('seen', n => ...)
```

## Programmatic API

The returned viewer can be used to programmatically control the viewer:

```js
viewer.flip_forward()
viewer.flip_back()
viewer.zoom()
```

## Single Page View

Finally, sometimes it makes sense to just show the book as a simple, scrollable view. To pass in only a `singlepage:true` option:

```js
flipbook(book, 'div-id', {singlepage:true}, (err, viewer) => ...
```

This will generate a series of canvases with `class="flipbook__page"` and `id="flipbook__pgnum_<n>"` that you can style using CSS. The single page view will raise the same `seen` event that the flipbook viewer does for tracking which pages the user actually flips through.

The single page viewer is currently experimental and very simple. It should work for many PDF's but is not optimized for handling PDF's with a large number of pages.


Enjoy!

------------

