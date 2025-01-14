---
layout: default
title: Security
description: How to configure league/commonmark against possible security issues when handling untrusted user input
redirect_from: /0.20/security/
---

Security
========

## HTML Input

**All HTML input is unescaped by default.**  This behavior ensures that league/commonmark is 100% compliant with the CommonMark spec.

If you're developing an application which renders user-provided Markdown from potentially untrusted users, you are **strongly** encouraged to set the `html_input` option in your configuration to either `escape` or `strip`:

### Example - Escape all raw HTML input:

~~~php
use League\CommonMark\CommonMarkConverter;

$converter = new CommonMarkConverter(['html_input' => 'escape']);
echo $converter->convertToHtml('<script>alert("Hello XSS!");</script>');

// &lt;script&gt;alert("Hello XSS!");&lt;/script&gt;
~~~

### Example - Strip all HTML from the input:
~~~php
use League\CommonMark\CommonMarkConverter;

$converter = new CommonMarkConverter(['html_input' => 'strip']);
echo $converter->convertToHtml('<script>alert("Hello XSS!");</script>');

// (empty output)
~~~

**Failing to set this option could make your site vulnerable to cross-site scripting (XSS) attacks!**

See the [configuration](/1.0/configuration/) section for more information.

## Unsafe Links

Unsafe links are also allowed by default due to CommonMark spec compliance.  An unsafe link is one that uses any of these protocols:

 - `javascript:`
 - `vbscript:`
 - `file:`
 - `data:` (except for `data:image` in png, gif, jpeg, or webp format)

To prevent these from being parsed and rendered, you should set the `allow_unsafe_links` option to `false`.

## Nesting Level

**No maximum nesting level is enforced by default.**  Markdown content which is too deeply-nested (like 10,000 nested blockquotes: '> > > > > ...') [could result in long render times or segfaults](https://github.com/thephpleague/commonmark/issues/243#issuecomment-217580285).

If you need to parse untrusted input, consider setting a reasonable `max_nesting_level` (perhaps 10-50) depending on your needs.  Once this nesting level is hit, any subsequent Markdown will be rendered as plain text.

### Example - Prevent deep nesting
~~~php
use League\CommonMark\CommonMarkConverter;

$markdown = str_repeat('> ', 10000) . ' Foo';

$converter = new CommonMarkConverter(['max_nesting_level' => 5]);
echo $converter->convertToHtml($markdown);

// <blockquote>
//   <blockquote>
//     <blockquote>
//       <blockquote>
//         <blockquote>
//           <p>&gt; &gt; &gt; &gt; &gt; &gt; &gt; ... Foo</p></blockquote>
//       </blockquote>
//     </blockquote>
//   </blockquote>
// </blockquote>
~~~

See the [configuration](/1.0/configuration/) section for more information.
