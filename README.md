# IansasciidocPreviewFrommarkdownpreviewnomd
altered version of Pulsar markdown-preview package that will recognise and preview asciidoctor .adoc files.

## Requisites:
Package  language-asciidoc must be installed and enabled. This shows the source code highlighted for asciidoc and it will allow the file type source.asciidoc to be selected.

The file type of the source file must be set to source.adoc either by looking at bottom right of window next to the encoding (likely UTF-8) and selecting source.asciidoc if it is not showing.
This file type can be set automatically by setting it as the default for .adoc files in pulsar config.cson file. (Open Settings pane and on left at bottom select config.cson). The entry at the top of the core section should contains something like this: (Other asciidoc extensions can be added.)
```
  core:
      customFileTypes:
        "text.plain": [
          "adoc"
          "asciidoc"
        ]
```

# To preview file        
To preview a file with cursor .adoc file press:
  <kbd>ctrl-alt-shift-a</kbd>  
(If the official pulsar mardown-preview package is enabled then <kbd>ctrl-shift-m</kbd> , will preview markdown files.)

What WORKS:
Asciidoctor files with .adoc or .asciidoc (& perhaps others if added) extensions are previewed.
Preview is synced to editor changes depending on setting-up.

By placing the cursor in adoc file it can be previewed in falkon browser by pressing <kbd>ctrl-alt-shift-f</kbd>. Internal target links will be functional. These are not allowed in the pulsar preview. External links will be operational.  Other links to local files may or may not  operate dependant on your setup. NOTE: falkon must be installed locally see https://www.falkon.org/

Can be saved as HTML by placing cursor in adoc preview and Right Click > Save as HTML. This file will open in a pane and can be previewed in pulsar with <kbd>ctrl-shift-h</kbd> where internal targets seem to function.

### What does NOT WORK:

Preview of <kbd> </kbd>

Test in spec directory do not function because they are the originals from pulsar markdown-preview. When I know more about how to write these that will change.

* NOTE: This package is based on the last pulsar v1.118.0  markdown-preview package before treesitter syntax highlighting was introduced for all the files in pulsar v1.119.0. 

## How IansasciidocPreviewFrommarkdownpreviewnomd functions:
Most of the code comes directly from pulsar markdown-preview. The main change is that when an adoc file is previewed the render function in render.js calls node asciidoctor.convert.js in renderAsciidoctor() instead of the original render(). render() is called with ```const domFragment = render(text, filePath)``` in render.js by exports.toHTML() & exports.toDOMFragment().
asciidoctor.convert is part of the node package asciidoctor.js. The big advantage in using this is that it is maintained by https://asciidoctor.org/ . It is a javascript translation of the Ruby asciidoctor.rb. This relieves pulsar of any maintenance.

The vast majority of the front matter Convert Options work as expected which includes, for example, style-sheet management, sourcemap, and standalone or embedded document. See https://docs.asciidoctor.org/asciidoctorj/latest/asciidoctor-api-options/. However, some of them are a bit tricky to use and the render renderAsciidoctor() function includes these in the call ```textToHTML = Asciidoctor.convert(text,{ 'standalone': true, 'safe':'safe',
'attributes': { 'linkcss': false , 'icons': 'font'}});``` to produce a working html file most of the time with an embedded style-sheet. Using asciidoctor.convert() means that none of these need to be handled by the pulsar package and their conversion coding is provided by the asciidoctor.org.

(At the moment there is a check that the file extension is .adoc but that will be removed soon. This is a remnant from the other similar package (but not developed properly yet) which will preview both md & adoc, IansasciidocPreviewFrommarkdownpreviewnomd Note terminal "nomd" no markdown.)

The new function in main.js ```openBrowserPopup()``` calls the browser falkon which will give an additional view of the links in the file. An alternative browser could be coded in. In the future this could be in the configSchema option.

WARNING: I am pretty new to js & pulsar package coding, so you will probably have to find how to make the package work for you: it certainly works for me on a daily basis maintaining websites with 100s of external & internal links.  But it does show proof of possibility that pulsar markdown-preview can be easily altered to preview asciidoc files. The other similar package IansasciidocPreviewFrommarkdownpreview, without the terminal "nomd" attempts to preview both markdown and asciidoc documents. It works, but it needs more development.

<hr><hr>

### Original README.me for pulsar markdown-package - not really for use in this package
# Markdown Preview package

Show the rendered HTML markdown to the right of the current editor using <kbd>ctrl-shift-m</kbd>.

It is currently enabled for `.markdown`, `.md`, `.mdown`, `.mkd`, `.mkdown`, `.ron`, and `.txt` files.

![iansasciidoc-preview-frommarkdownpreviewnomd](https://cloud.githubusercontent.com/assets/378023/10013086/24cad23e-6149-11e5-90e6-663009210218.png)

[click on this link to hash-my-multi-word-header](#my-multi-word-header)

<p><a href="#my-multi-word-header">click on this href= link to hash-my-multi-word-header</a></p>

## Customize

By default Markdown Preview uses the colors of the active syntax theme. Enable `Use GitHub.com style` in the __package settings__ to make it look closer to how markdown files get rendered on github.com.

![iansasciidoc-preview-frommarkdownpreviewnomd GitHub style](https://cloud.githubusercontent.com/assets/378023/10013087/24ccc7ec-6149-11e5-97ea-53a842a715ea.png)

To customize even further, the styling can be overridden in your `styles.less` file. For example:

```css
.iansasciidoc-preview-frommarkdownpreviewnomd.iansasciidoc-preview-frommarkdownpreviewnomd {
  background-color: #444;
}
```

## Syntax Highlighting Language Identifier

While a verbose specification of Markdown, mostly, ensures the content of Markdown will look the same everywhere it's shipped, the same isn't true of code block language identifiers.

A "code block language identifier" is the string you use to tell the Markdown renderer what code is inside a code block of your Markdown document.

Nearly every Markdown rendering system supports different strings to specify your language than each other. `iansasciidoc-preview-frommarkdownpreviewnomd` has implemented several valid Language Identifier systems to help ensure that your Markdown will look the same no matter where you publish it!

### My Multi Word Header

In the settings, you can select from a list of different popular Language Identification systems that can then be used in your code blocks. This means that they will still be valid when shipping them to whatever platform of your choice.

Currently, `iansasciidoc-preview-frommarkdownpreviewnomd` supports the following:

  * Linguist: Used by GitHub (Previously the default and only language identification system)
  * Chroma: Used by CodeBerg/Gitea/Hugo/Goldmark
  * Rouge: Used by GitLab/Jekyll
  * HighlightJS: Used by Markdown-IT/Pulsar Package Website


Of course, not all Markdown content is destined to be shared, `iansasciidoc-preview-frommarkdownpreviewnomd` even lets you specify custom Language Identifiers to be used within your Markdown code blocks.

The setting `Custom Syntax Highlighting Language Identifiers` lets you define a list of custom language identifiers that match up to languages available within your Pulsar installation

For example, if you wanted to highlight your code like JavaScript by just using `j` as your Code Block language Identifier and `p` to use Python Syntax Highlighting, you could add the following to this setting:

```
j: source.js, p: source.python
```

And that's it, now anytime you use that language identifier it will be highlighted exactly the way you want. Of course your preference of language identification system will still be used, in addition to your custom list.
