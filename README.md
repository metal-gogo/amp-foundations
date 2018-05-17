# amp-foundations

This folder contains the source code for a Google I/O 2016 codelab. In addition to that it has the incremental changes to convert this simple [article.html](/article.html) into an AMP compatible [article.amp.html](/article.amp.html).


### License

```
Copyright 2016 Google, Inc.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements. See the NOTICE file distributed with this work for
additional information regarding copyright ownership. The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License. You may obtain a copy of
the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
License for the specific language governing permissions and limitations under
the License.
```


### Lessons learned

#### AMP design principles:

- User Experience > Developer
- Experience > Ease of Implementation.
- Don’t design for a hypothetical faster future browser.
- Don’t break the web.
- Solve problems on the right layer.
- Only do things if they can be made fast.
- Prioritise things that improve the user experience – but compromise when needed.
- No whitelists.

#### Include the AMP library

It's mandatory to include the AMP library at the buttom of the `<head>` tag.

```html
<script async src="https://cdn.ampproject.org/v0.js"></script>

```

The AMP library includes an [AMP validator](https://www.ampproject.org/docs/fundamentals/validate) that will tell you if there is anything that keeps your page from being a valid AMP document. Enable it by adding the next identifier to your document URL `#development=1`.

#### Needed `<meta>` tags in

You must add the next lines as the first two lines of the `<head>` tag:

```html
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">
```

> NOTE - The values specified for width and minimum-scale are the required values in AMP. Defining initial-scale is not mandatory but it’s a commonly included in mobile web development and it's recommended.

#### Include the mandatory canonical link

```html
<link rel="canonical" href="/article.html">
```

> NOTE — You can create a standalone canonical AMP page. The canonical link is still required, but should point to the AMP file itself.

#### Specify the AMP attribute

```html
<html ⚡ lang="en">
<!-- or -->
<html amp lang="en">
```

> NOTE — Although specifying the ⚡ is the recommended approach, it's also possible to use the amp attribute in place of the ⚡ attribute.

#### Replace external stylesheets

Not only is inline styling required but there is a file size limit of 50 kilobytes for all styling information. All styling information must be inside a `<style amp-custom></style>` tag.

> NOTE - You should use CSS preprocessors such as SASS to minify your CSS before inlining the CSS in your AMP pages.
> 
> IMPORTANT — You can only have one style tag in your entire AMP document. If you have several external stylesheets referenced by your AMP pages, you will need to collate these stylesheets into a single set of rules. To learn what CSS rules are valid in AMP, read Supported CSS.

#### Exclude third-party JavaScript

While stylesheets can be reworked relatively easily with AMP by inlining the CSS, the same is not true for JavaScript.

> The tag 'script' is disallowed except in specific forms.

In general, scripts in AMP are only allowed if they follow two major requirements:

1. All JavaScript must be asynchronous (i.e., include the async attribute in the script tag).
2. The JavaScript is for the AMP library and for any AMP components on the page.

This effectively rules out the use of all user-generated/third-party JavaScript in AMP except as noted below.

> NOTE — The only exceptions to the restriction on user-generated/third-party scripts are:
>  1. Script that adds metadata to the page or that configures AMP components. These will have the type attribute `application/ld+json` or `application/json`.
> 2. Script included in iframes. Including JavaScript in an iframe should be considered a measure of last resort. Wherever possible, JavaScript functionality should be replaced by using [AMP components](https://www.ampproject.org/docs/reference/components).

#### Include AMP CSS boilerplate

Every AMP document requires the following AMP boilerplate code at the bottom of the `<head>` tag:

```html
<style amp-boilerplate>body{-webkit-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-moz-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-ms-animation:-amp-start 8s steps(1,end) 0s 1 normal both;animation:-amp-start 8s steps(1,end) 0s 1 normal both}@-webkit-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-moz-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-ms-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-o-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}</style><noscript><style amp-boilerplate>body{-webkit-animation:none;-moz-animation:none;-ms-animation:none;animation:none}</style></noscript>
```

The `<style amp-boilerplate>` tag initially hides the content of the body until the AMP JavaScript library is loaded, then the content is rendered. AMP does this to prevent unstyled content from rendering, also known as Flash Of Unstyled Content (FOUC). 

The `<noscript>` tag reverts this logic if JavaScropt is disabled in the browser providing a basic **graceful degradation** for this.



