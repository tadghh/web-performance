# Performance Optimization Strategy in 2023 - [Source](https://paper.dropbox.com/doc/Performance-Optimization-Strategy-in-2023-qWcr7orx2cEWHpLqoLeTC)
Compiled by [Vitaly Friedman](https://www.smashingmagazine.com/). https://smashed.by/perf-strategy

## Get a big picture
- Run a test with [Yellow](https://yellowlab.tools/) [](https://yellowlab.tools/)[lab Tools](https://yellowlab.tools/)
- Check the state of things in [CrUX](https://g.co/chromeuxdash)
- Check [resource loading priorities](https://docs.google.com/document/d/1bCDuq9H1ih9iNjgzyAL0gpwNFiEP4TZS-YLRp_RuMlc/edit#) in Chrome’s Network panel + [differences](https://twitter.com/programmingart/status/1610285187845210113)
- Check [Bulk Core Web Vitals](https://www.experte.com/web-vitals)
- Show [Core Web Vitals overlay](https://www.debugbear.com/docs/metrics/core-web-vitals#:~:text=Core%20Web%20Vitals%20Overlay%E2%80%8B&text=You%20can%20enable%20the%20overlay,Layout%20Shift%20for%20the%20page.) in Chrome DevTools
- Make [good use of DevTools](https://paper.dropbox.com/doc/Front-End-2021-DevTools--BMVWlqjD5LtC6V_n~bayhEo4Ag-5fG4p9hp5PcSyaC18vuZJ)
- Check the performance panel in Chrome
- Run a test for [HTML site analyzer](https://www.debugbear.com/html-size-analyzer)
- Run a test for resource hints with a [resource hints validator](https://www.debugbear.com/resource-hint-validator)
- Check the [waterfall in WebPageTest](https://nooshu.com/blog/2019/10/02/how-to-read-a-wpt-waterfall-chart/)
- Check the [Search Console](https://search.google.com/search-console/core-web-vitals?resource_id=https%3A%2F%2Fwww.smashingmagazine.com%2F&hl=en-GB), [PageSpeed Insights](https://pagespeed.web.dev/) and [Lighthouse Metrics](https://lighthouse-metrics.com/)
- Check the [carbon footprint impact](https://www.websitecarbon.com/)
- Check the [third-parties](https://csswizardry.com/2018/05/identifying-auditing-discussing-third-parties/) with [Requestmap](https://requestmap.webperf.tools/)
- Consider ways to [accelerate JavaScript in the browser](https://jonathandinu.com/blog/accelerating-js/)
- Check what browsers we are supporting, and how
- Check: [Can Include](https://caninclude.glitch.me/), [CanIEmail](https://www.caniemail.com/), [WhatTheTag](https://whatthetag.com/#/), [WhoCanUse](https://whocanuse.com/), [Which Icon Is That](https://www.whichiconisthat.com/), [IsItAccessible?](https://isitaccessible.dev/)


## 1. Clean up and reorder the <head>
- Turn on Brotli compression
- Use [fewer but larger bundles](https://csswizardry.com/2023/10/the-three-c-concatenate-compress-cache/?ref=sidebar) for fast delivery
- Reduce the markup of [SVG favicons](https://evilmartians.com/chronicles/how-to-favicon-in-2021-six-files-that-fit-most-needs)
- [Pre-connect](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel/preconnect) to critical domains
- Preloading critical fonts
- Not loading fonts for users with `saveData` on and `prefers-reduced-data` on
- [Preload critical files](https://web.dev/preload-critical-assets/)
- [Measure timings](https://developer.mozilla.org/en-US/docs/Web/API/Performance/mark) with Local Overrides
- [Validate your resource hints](https://www.debugbear.com/resource-hint-validator)
- Use [ct.css](https://csswizardry.com/ct/) and [Capo.js](https://github.com/rviscomi/capo.js) to sort the <head>
    
![Studied and analyzed by Harry Roberts](https://paper-attachments.dropbox.com/s_8C6AF3BE8876E1B3545A7D97DA44F9EEBF7D84B933DD8A4D22D122A4FC35CA55_1643989412686_image.png)



## 2. Manage critical CSS and CSS loading
- Define and inline critical CSS per template, but [beware of implications](https://csswizardry.com/2022/09/critical-css-not-so-fast/)
- Create *critical.css* and *non-critical.css* for each template
- Use `outline: 3px solid red` to check layout shifts
- Remove all external SVGs (inlined into CSS)
- Adjust [Browserslist](https://browserslist.dev/?q=bGFzdCAyIHZlcnNpb25z) to avoid legacy browsers (IE)
- Use the [print stylesheet loading trick](https://www.filamentgroup.com/lab/load-css-simpler/) to load non-critical CSS async


## 3. Improve Web Font Loading
- Always [self-host fonts](https://csswizardry.com/2019/05/self-host-your-static-assets/) on your own servers
- Check [high-performance web font loading](https://www.industrialempathy.com/posts/high-performance-web-font-loading/) and [best practices for fonts](https://web.dev/font-best-practices/)
- Use `[font-display: optional](https://glitch.com/~font-display)` (or `swap`) for fast rendering
- [Group repaints to avoid reflows](https://www.smashingmagazine.com/2021/01/smashingmag-performance-case-study/#changing-the-web-font-loading)
- [Subsetting fonts](https://everythingfonts.com/subsetter) (alternatively with [Glyphhanger](https://github.com/filamentgroup/glyphhanger))
    - $, §, ×, ", ',’,“,Á,Â, É,Í, Ó, é, é, é, â, î , ô, ñ, ö, ü, ä, ć, ç, ß, á, é, í, ó, ú, š, ž, å, £, €, ÷, –,  —, “, ”, ć, ¢, %, ‰, …, ¶
- Move to Latin-1 Supplement
- Subset variable fonts with [Slice](https://github.com/source-foundry/Slice)
- Match font fallback with web fonts via [Fallback Font Generator](https://screenspan.net/fallback)


![](https://paper-attachments.dropbox.com/s_544D91DECC81533ED1FE7801C94725F22BBF7773E2627641694C4B3F383BF32D_1615404558762_Screen+Shot+2021-03-10+at+8.28.26+PM.png)



## 4. Optimize the rendering for LCPs
- [Maximize image loading performance](https://www.industrialempathy.com/posts/image-optimizations/) with AVIF
- Run an [image analysis tool](https://webspeedtest.cloudinary.com/) for images
- Add `preload` for LCPs (usually images or large headings)
- Preload responsive images with [img srcset and image](https://www.stefanjudis.com/today-i-learned/how-to-preload-responsive-images-with-imagesizes-and-imagesrcset/) [](https://www.stefanjudis.com/today-i-learned/how-to-preload-responsive-images-with-imagesizes-and-imagesrcset/)[sizes](https://www.stefanjudis.com/today-i-learned/how-to-preload-responsive-images-with-imagesizes-and-imagesrcset/)
- Choose the [lightest SQIP option](https://axe312ger.github.io/sqip/) or [BlurHash](https://github.com/woltapp/blurhash)
- Add Early hints if the server needs “server think time” to return pages
- Optimize images for [high-density displays](https://browserslist.dev/?q=bGFzdCAyIHZlcnNpb25z)
- By default all non-critical images include `[loading=](https://web.dev/browser-level-image-lazy-loading/)``["](https://web.dev/browser-level-image-lazy-loading/)``[lazy](https://web.dev/browser-level-image-lazy-loading/)``["](https://web.dev/browser-level-image-lazy-loading/)` and `[decoding="async"](http://www.js-craft.io/blog/what-does-the-html-image-decoding-async-attribute-do-and-how-can-it-help-us-to-improve-performance/)` ([what decoding means](https://www.tunetheweb.com/blog/what-does-the-image-decoding-attribute-actually-do/))
- All images should [have](https://www.smashingmagazine.com/2020/03/setting-height-width-images-important-again/) `[height](https://www.smashingmagazine.com/2020/03/setting-height-width-images-important-again/)` [and](https://www.smashingmagazine.com/2020/03/setting-height-width-images-important-again/) `[width](https://www.smashingmagazine.com/2020/03/setting-height-width-images-important-again/)` [defined](https://www.smashingmagazine.com/2020/03/setting-height-width-images-important-again/)
- Above-the-fold images should include `loading=``"``eager``"`
- Use [facades for videos](https://web.dev/third-party-facades/)
- Base64 encoding LCP to improve perf on mobile (AVIF and/or JPEGs) (*maybe*)
- Always convert [GIFs to MP4](https://cloudconvert.com/gif-to-mp4)
- Check LCPs in Chrome’s performance panel


## 5. Deferring/delaying JS and third-parties
- Find your [biggest JavaScript offenders](https://paper.dropbox.com/doc/Performance-Optimization-Strategy-in-2024--CCaUa6EBjXKjwR9uyr6FkRJ8Ag-qWcr7orx2cEWHpLqoLeTC)
- Find the [true size of an npm package](https://pkg-size.dev/)
- Define priorities for critical and non-critical JS
    - critical JS needs to be coming early / sync or async
    - non-critical JS needs to be delayed as far as possible
- Use [SSR + progressive hydration](https://www.patterns.dev/posts/progressive-hydration/)
- [Delay service worker installation](https://twitter.com/patmeenan/status/1367849616457228294) until the `onload` event
- Add lazy loading and async decoding to CodePen, Vimeo, YouTube
- Use [facades](https://web.dev/third-party-facades/) for third-party resources
- Replace large libraries like Moment.js with [smaller counterparts](https://blog.logrocket.com/4-alternatives-to-moment-js-for-internationalizing-dates/) 
- Consider moving third-parties to a web worker with [Partytown](https://github.com/BuilderIO/partytown)
- Use tools like [Instant.page](https://instant.page/) for early prefetching
- [Delay JavaScript](https://blog.speedvitals.com/delay-javascript/) until interaction
- Use a [service worker](https://jakearchibald.github.io/isserviceworkerready/resources.html) to reliably cache assets
- Break down long tasks with [isInputPending](https://developer.chrome.com/articles/isinputpending/) and `[setTimeout](https://web.dev/optimize-inp/)`
- Follow along with framework-specific optimizations
- Get the [Cache-Control directives](https://csswizardry.com/2019/03/cache-control-for-civilians/) in order
- Consider [new JavaScript and TypeScript features](https://betterprogramming.pub/all-javascript-and-typescript-features-of-the-last-3-years-629c57e73e42)

