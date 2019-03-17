# Ubqt-flavored markdown

Below is a list of markdown elements supported by ubqt, as well as a rough example of how they may be displayed by a client.

## From Common

` > Block quote some text `

> Block quote some text

```
# Header
## Header 2 
### Header 3
#### Header 4
##### Header 5
```

# Header
## Header 2
### Header 3
#### Header 4
##### Header 5

```
 - list
 - of items

 * list
 * of items

 - list
   - sublist
   - sublist
 - first list

 1. Ordered
 2. List
 3. Of items
```
 - list
 - of items

 * list
 * of items

 - list
   - sublist
   - sublist
 - first list

 1. Ordered
 2. List
 3. Of items

\` inline code \`
`inline code`

\`\`\`
multiline
code
\`\`\`

`~~strikethrough~~`
~~strikethrough~~

## Ubqt

Images:
`![alt text of image](/path/to/image "Image text")`

Links:
`[text of a link](https://someurl.com)`

Colored text:
`%[Some text to be colored](colorname)`

Checkboxes:
`[x]`
`[ ]`

## Future

As ubqt grows and is used for more purposes, this list will undoubtedly change.
Cleanmark is considered to provide an API reference to the current state of the art for ubqt markdown, and any breaking changes will constitute a release of cleanmark. All changes will discussed in the release notes.