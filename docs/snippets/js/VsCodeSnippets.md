---
title: Docusaurus Dev
sidebar_position: 101
sidebar_label: Doc Code
---



# Docusaurus Dev


##  Callouts

:::tip[My tip]

Use this awesome feature option

:::

:::danger[Take care]

This action is dangerous

:::



```markdown title="callout.md"
:::tip[My tip]

Use this awesome feature option

:::

:::danger[Take care]

This action is dangerous

:::
```

**VsCode**

```javascript title="nwp.code-snippets"
"docusaurus_callout": {
  "prefix": "nwp-docu-callout",
  "body": [
    ":::tip[${1:Titel}]",
    "",
    "${2:Text}",
    "",
    ":::",
    "",
    ":::danger[${1:Titel}]",
    "",
    "${2:Text}",
    "",
    ":::"
  ],
  "description": "docusaurus_callout"
}
```

## Header

```yaml title="header.yaml"
---
title: VsCode
sidebar_position: 101
sidebar_label: VsCode
---
```

**VsCode**

```javascript title="nwp.code-snippets"
"docusaurus_header": {
  "prefix": "nwp-docu-header",
  "body": [
    "---",
    "title: ${1:Titel}",
    "sidebar_position: ${2:number}",
    "sidebar_label: ${1:Titel}",
    "---"
  ],
  "description": "docusaurus_header"
}
```

## Categorie

```json title="_category_.json"
{
  "label": "Categorie",
  "position": 10,
  "link": {
    "type": "generated-index",
    "description": "Info"
  }
}
```

## TOC

```markdown title="_index.md"
---
title: "TOC"
draft: false
noTitle: false
fullWidth: false
anchors: true
---

{{<cta for="funnel">}}

<div id="table-of-contents"></div>
```


## Page


```markdown
---
title: VsCode
sidebar_position: 101
sidebar_label: VsCode
---

## Title
```


## Codeblock


~~~
```html title="index.html"
<!-- content --> 
```
~~~





