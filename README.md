# remark-github-beta-blockquote-admonitions

remark plugin to add support for [GitHub beta blockquote-based admonitions](https://github.com/github/feedback/discussions/16925)

## Get Started

```js
import plugin from 'remark-github-beta-blockquote-admonitions'

const options = {} // Plugin options
const html = String(
  await remark().use(remarkParse).use(plugin, options).use(remarkRehype).use(rehypeStringify).process(`\
# Admonitions
> **Note**
> test
`)
)
/* The output HTML will be:
<h1>Admonitions</h1>
<blockquote class="admonition">
<p><strong class="admonition-title">Note</strong>
test</p>
</blockquote>
 */
```

## Config

```ts
export interface Config {
  classNameMaps: {
    block: string | string[] | (title: string) => (string | string[]) // Classes the <blockquote> block should be added with
    title: string | string[] | (title: string) => (string | string[]) // Classes the <strong> title should be added with
  }
  titleFilter: string[] | (title: string) => boolean // Which title texts in <strong> should make the block considered as admonitions
  titleLift: boolean // When enabled, the <strong> element will be moved from <p> children to <blockquote> children with <p> wrapped, like the structure of MkDocs admonitions, otherwise no extra actions
  titleLiftWhitespaces?: (whitespaces: string) => string // When titleLift is enabled, after <strong> is moved, the function defines what the whitespaces following the <strong> will be converted to. When not provided, remove these whitespaces. You may rarely need to set the option unless want to strictly control the syntax tree.
}
export const defaultConfig: Config = {
  classNameMaps: {
    block: 'admonition',
    title: 'admonition-title',
  },
  titleFilter: ['Note', 'Warning'],
  titleLift: false,
}
```

## How To

### Collaborate with MkDocs admonitions?

[MkDocs admonitions](https://www.markdownguide.org/tools/mkdocs/#using-admonitions) is just [Python-Markdown admonitions](https://python-markdown.github.io/extensions/admonition/) which is from [rST-style admonitions](https://docutils.sourceforge.io/docs/ref/rst/directives.html#specific-admonitions)

TODO

## Implementation

Since GitHub beta blockquote-based admonitions are backward compatible in Markdown, things are simple, which are just to visit the matched elements in the `remark-parse` parsed syntax tree to add `remark-rehype` recognizable classes

## License

Copyright 2022 myl7

SPDX-License-Identifier: Apache-2.0
