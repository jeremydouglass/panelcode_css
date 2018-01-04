
```panelcode
r3.gray9 + c2.r2.gray9, 1 + 1z
  {::: small dropcap }
```

# Panelcode: layout markup and visualization

_Jeremy Douglass_
_2017-12_

<nav><ul>
<li><a href="#basics-rows-and-columns">Basics</a></li>
<li><a href="#reading-a-panelcode-image">Reading</a></li>
<li><a href="#spans">Spans</a></li>
<li><a href="#images">Images</a></li>
<li><a href="#sizes">Sizes</a></li>
<li><a href="#spreads">Spreads</a></li>
<li><a href="#sequences">Sequences</a></li>
<li><a href="#spaces">Spaces</a></li>
<li><a href="#ambiguity-and-complexity">Ambiguity</a></li>
<li><a href="#shapes">Shapes</a></li>
<li><a href="#fill-color-and-style">Fill</a></li>
<li><a href="#flow">Flow</a></li>
<li><a href="#direction">Direction</a></li>
<li><a href="#bleeds">Bleeds</a></li>
<li><a href="#code-style">Code Style</a></li>
</ul></nav>

```panelcode
  1; 2|3; 1_1|1_1_1; 1+c3_c3+1; 2_2|3_3_3
  {::: mini nocode }
```

**Panelcode** is a minimal markup language for describing visual compositions as abstract layouts.

```panelcode
  1 {::: mini dropcap }
```

Use Panelcode to **quickly encode visual layouts for search and visual exploration**. Panelcode is optimized to be quickly and easily writable, readable, and editable by hand, with an emphasis on speed, concision, and human judgement. The tools were developed with a particular focus on compositions made of panels such as those occuring in comics, graphic novels, newspapers, magazines, websites, et cetera, and the first test cases have been performed on comics.

```panelcode
  2 {::: mini dropcap }
```

Panelcode supports multiple encoding schemes, but examples in this document use its base scheme: gridcode. Panelcode also has many extensions and shorthands, a number of which are touched on here: markup for blank spaces, annotations, unencoded regions, panel shapes, and bleeds. Finally, Panelcode has renderers to create multiple outputs, including SVG (Scalable Vector Graphics), scaffolding TEI-XML, and HTML-Tables.

```panelcode
  3 {::: mini dropcap }
```

**This document focuses on the gridcode HTML5-CSS3 renderer**, which renders panelcode as either HTML5 web content or stand-alone SVG image files. It is an illustrated tour of some of Panelcode's rendering features. Rather than walking through the complete language syntax, or discussing the design of the parser, it instead focuses on the gridcode renderer and walks through an illustrated tour of basic examples of Panelcode and what you can do with it: describe, summarize, compare, annotate, and vizualize layouts. This renderer supports:

-  rows and columns
-  multi-row / columns spans
-  empty spaces
-  multi-panel regions
-  transforms (skew, scale, rotate)
-  fills (color, texture)
-  sizes (small, thumb, mini)
-  media formats (abstract, comic, strip)
-  right-to-left rendering
-  bleeds

This page was rendered from a Markdown document with inline Panelcode. Throughout the page you will find rendered examples -- hover over them to access resizing controls and to unfold the code view, showing the code which rendered the gallery

## Basics: rows and columns

```panelcode
  1
; 2 | 3
; 1_1 | 1_1_1
; 1_2 | 1_2_3 | 4_3_2_1 | 6_4_3_5_2_1
; 2_2 | 3_3_3
  {::: mini nocode }
```

```panelcode
r3.gray9 + c2.r2.gray9, 1 + 1z
  {::: mini dropcap }
```

Panelcode is a very efficient shorthand when encoding simple page layouts. Encodings may be written by hand and then parsed and programmatically processed for many purposes: searching, analyzing, rendering graphics and creating information visualizations, or producing scaffolds for layout editing or marking up pages with metadata.

A number in Panelcode signifies a simple row of panels. A single number ("`3`") is a valid Panelcode string that describes a layout with a single row of 3-columns.

```panelcode
  1
; 2
| 3
  {::: thumb }
```

The Panelcode strings "`1`" "`2`" and "`3`" appearing at the bottom of each glyph are not page numbers -- each number describes the row of panels in each layout.

```panelcode
  4
  {::: comicstrip-daily mini dropcap }
```

In Panelcode, a sequence of panels is the basic unit of description: A "`4`" means four panels, as in a typical daily newspaper comic, for example. Those panels proceed in whatever typical reading order they were encoded in -- left-to-right rows and top-to-bottom columns, in this case, but Panelcode input and/or output can be automatically mirrored for e.g. manga, scanlations and imported reprints.

```panelcode
  1_1
| 1_1_1
  {::: thumb }
```

Rows are joined into layout stacks using underscores: `1_1` or `1_1_1`.

The default row operator "`_`" (the underscore) is the glue used to build complex pages by connecting of simple rows. While `3` described three horizontal panels, `1_1_1` describes three vertical panels. This can be made more concise with one of the shorthand extensions, but it reflects a fundamental aspect of panelcode "gridcode"-style encoding design: it is organized by a primary reading order (top-to-bottom) and a secondary reading order (in this case, left-to-right). In fact, panels connected by the column operator "`+`" are commonly collapsed into row counts: the horizontal panelcode layout `3` could also be writtn `1+1+1`. (The vertical panelcode layout `1_1_1` can -- with an extension -- be written `3v`.)

Combine different row counts into layouts: `1_2`, `1_2_3` et cetera.

```panelcode
  1_2
| 1_2_2
; 4_3_2_1
| 6_5_4_3_2_1
  {::: small }
```

When the row operator is used to combine rows then each row may have different panel counts, and every row is assumed by default to be full-width with its own unique panel dimensions.	This is fundamentally unlike a spreadsheet, HTML &lt;table&gt;, or other tabular data presentation system. In	such systems columns are fixed, and rows must be marked of in units or multiples of those columns. In order to create a `1_2_3_4` on a spreadsheet requires 12 columns, and each row must be expressed in multiples of those columns.

These fixed column constraints are essentially the limitations of attempting to express design grids in tabular data presentation formats. By contrast Panelcode is based on a flexible design grid paradigm -- and in fact uses the emerging CSS flex / flexbox and CSS grid standards for this renderer, which are likewise based on an evolution of web design away from tabular data and towards design grids.

> [Although note that the original Panelcode HTML-Tables renderer accomplished this by making every row or rowgroup its own independent table -- such workarounds are often painful, but not impossible.]

```panelcode
  2_2
| 3_3_3
  {::: small }
```

Combine 2_2 or 3_3_3 row counts into stacks to form grids.

```panelcode
  8_8_8_8_8_8_8_8
```

This is not to say that spreadsheet-style grids are impossible or even difficult in Panelcode. A chessboard-sized grid can be described by an `8_8_8_8_8_8_8_8` -- or, using multiplier shorthand, an `8*8`.


## Reading a Panelcode Image

Panelcode visualizations may have a different appearance and different elements depending on the renderer, however there are some basics that are common to the Panelcode concepts. Consider this glyph:

```panelcode
1_2
{: label='1_2'}
```

A Panelcode glyph is an abstract representation of a layout -- like a design wireframe. It may describe a page, a screen, or some other medium -- and, in fact, it may describe many different objects as having the same abstract composition. In fact, this abstraction is the key to comparing disperate layouts. (To say that two graphic novel pages have the same glyph is less like saying two songs have the same notes or two poems have the same rhymes, and more like saying two songs have the same rhythm, or that two poems have the same rhyme scheme.)

The margins of a Panelcode glyph may display additional information, including:

-  The Panelcode string used to generate the glyph.
-  A title / caption.
-  Reference metadata -- such as a graphic novel volume / issue and page number, or a strip publication date or webcomic series number.
-  Layout metadata - such as a count of panels, or a categorization or tagging of contents.
-  Textual or visual annotations within specific panel regions. By default panelcode renders panel id numbers

Depending on the renderer this box-model of a page or screen may support empty area and unencoded regions (see more below). It may also support the use of encoded metadata to styling panels with different textures or colors etc. to indicate their nature -- e.g. full page spread -- or annotations of contents -- e.g. incidents of speech (see more below). Panelcode glyphs are designed to be viewed en masse in large collections or from a distance, but individaul panels may include numbers that indicate the encoding order -- which is often (but not always) the expected reading order for the panels in a given layout.

Panelcode glyphs appear by default on an abstract unit square -- for example, it is neither the vertical rectangle of the comic book nor the horizontal rectangle of the newspaper cartoon strip. This is to emphasize the concept that a particular geometric arragement of panels might refer to either medium, or both, or something.Panelcode glphys can also be styled to media-specific dimensions -- either the dimensions of a media genre (e.g. a contemporary US comic), a specific object (e.g. _Mouse Guard_), or a mixed media collection (e.g. the artifacts of _Building Stories_).

```panelcode
  c3.grayC + 1.grayB
+ c2.grayA + c2.gray9
+ 1.gray8 + c3.gray7
  {: w4 label='Abstract unit square (1:1)' }
; c3.grayC + 1.grayB
+ c2.grayA + c2.gray9
+ 1.gray8 + c3.gray7
  {: w4 comicstrip-sunday-half label='Newspaper comicstrip ("Sunday half" aspect)'}
; c3.grayC + 1.grayB
+ c2.grayA + c2.gray9
+ 1.gray8 + c3.gray7
  {: w4 comicbook-us label='Comicbook page (US aspect)' }
```

The glyph for `c3+1_2_1+c3` rendered on a unit square, then rerendered while tagged as a newspaper comicstrip (14x10 aspect ratio, Sunday half-page) or as a comicbook page (11x17 aspect ratio, contemporary US comic). Panelcode output may visually match the specific media it was encoded from, or it may also abstract page dimensions when comparing layouts in mixed media collections.


## Spans

Spans are a simple way of creating more complex layouts than combining rows will allow. A span specifies that a panel is proportionately wider that other in its row (column span) or that a panel occupies multiple rows in a rowgroup (row span). A panel may have both its column span and row span specified. In Panelcode spans are indicated with the `c` and `r` attributes, which are attached to panels as suffixes.

### Column  Spans

```panelcode
  2
; 1+c2 | c2+1
; 2_2
; 1+c2 _ c2+1 | c2+1 _ 1+c2
  {::: mini nocode }
```

Columns are the basic unit of panelcode: `1`, `2`, `3` etc. is the code for a single row of 1, 2, or 3 columns. Panels in simple rows are commutative: a row of `3` = `2+1` = `1+1+1` -- all describe the same layout.

```panelcode
  3
; 2 + 1
; 1 + 1 + 1
  {::: thumb }
```

However, this assumes that each panel is of the same proportional width. When this is not the case, the `c` attribute plus a number argument can be added to individual panels to define their proportional width in units. The panelcode `2` is instead written as an expanded group of `1 + 1` of individual panels. One of those panels is annotated with the suffix `.c2` -- this sets the column span equal to 2, making the second column twice as wide as the first (and its default span of c1).

```panelcode
  1 + 1.c4     {: label='1:4' }
; 1.c2 + 1.c3  {: label='2:3' }
  {::: small }
```

Panelcode attributes are each attached to the panel number with a `.` -- several attributes may be chained, in any order. When the number is `1`, the attribute affects only that panel. When the number describes a simple group, the attribute applies to every panel in that group.

```panelcode
  2.c2 + 1
_ 3.c2 + 1 
  {::: small }
```

A common use of column spans in comic layouts is to create staggered rows.

```panelcode
  1 + c2 _ c2 + 1
| c2 + 1 _ 1 + c2
  {::: small }
```

Note that writing panelcode in a compact style, the initial `.` may be ommitted: `1c2`. Further, the panel number `1` may be ommited -- a bare attribute `c2` implicitly modifies a single panel.

```panelcode
  1 + 1.c2
; 1 + 1c2
; 1 + c2
  {::: thumb }
```


### Row Spans

```panelcode
  r2+1,1 | 1+r2,1
; 1_2 | 2_1
; 1_2 | 2_1 | r2+1,1 | 1+r2,1
; c2.r2+1, 1
  {::: mini nocode }
```

Rows are also fundamental to panelcode -- multiple simple rows may be assembled into a single layout with the row delimiter `_`, e.g. `1_2`. For panels that cross multiple rows, however, simple rows must instead defined as complex rowgroups using the `,` rowgroup delimiter, and a panel must be marked with the rowspan attribute `r`  plus the number of rows to span -- `1.r2`, `1.r3` et cetera. As with column spans, the `1.r2` row span may be shortened to `1r2` -- or simply `r2`. 

One of the most common rowgroups is the simple left-hand rowspan: `r2+1,1`. 

```panelcode
  r2 + 1, 1
; 1 + r2, 1
  {::: small }
```

This looks complex when compared with the similar layouts `1_2` or `2_1`. 

```panelcode
  r2 + 1, 1
; 1_2
| 2_1
  {::: mini }
```

Because the long panel is oriented horizontally rather than vertically these layouts can be described without rowspan notation -- but rowgroup delimiters introduce a lot of flexibilty to build complex layouts.

The `+` delimiter continues along the same row, while the `,` delimiter acts like a carriage return, leaving the row and proceeding to the next available blank space below. For example, in `r5 + r4, 1`, the next available space  for the `1` is on the 5th row.

```panelcode
  r5 + r4, 1
  {::: small }
```

> [This concept of a "rowspan" argument is present in early HTML Tables, and also incorporated into CSS3 Grid. Panelcode uses these concepts in part because they are longstanding traditions in rendering tabular data, and in part because shared concepts make it easier to parse a panecode string into a straightforward sequence of HTML or CSS elements that can directly render the described layout.]

Column span and row span attributes may also be combined, as in the multi-row, multi-column panel in this example:

```panelcode
  c2.r3 + r2, 1
  {::: small }
```

These C-shaped compositions of 3 panels are uncommon in newspaper comics, but are an extremely common building block of graphic novel and comicbook pages. A typical comicbook might composite this panelgroup along with one or two simple rows:

```panelcode
  3
_ r2+1, 1
_ 1
  {::: small }
```


## Images

```panelcode
  1_1+r2,1 {: img='images/174.png'}
; 1_1+r2,1 {: img='images/174.png' iover }
; 1_1+r2,1 {: img='images/174.png' ibefore }
; 1_1+r2,1 {: img='images/174.png' iafter }
  {::: comicbook-us mini nocode }
```

Panelcode can also specify methods for companion image display. Specfing an img path displays that image on top of the layout. The `ishow` and `ihide` attributes make the image interactively show or hide on hover.

```panelcode
  1_1+r2,1 {: img='images/174.png'}
; 1_1+r2,1 {: img='images/174.png' ishow }
; 1_1+r2,1 {: img='images/174.png' ihide }
  {::: comicbook-us small }
```

Use `iover` to style the image as semi-opaque (onion skin). The “over” image layer can also be made interactive with `ishow` or `ihide` -- from barely opaque to fully shown, or from barely transparent to fully hidden.

```panelcode
  1_1+r2,1 {: img='images/174.png'}
| 1_1+r2,1 {: img='images/174.png' ishow }
| 1_1+r2,1 {: img='images/174.png' ihide }
; 1_1+r2,1 {: img='images/174.png' iover }
| 1_1+r2,1 {: img='images/174.png' iover ishow }
| 1_1+r2,1 {: img='images/174.png' iover ihide }
  {::: comicbook-us thumb }
```

Images can also be displayed in a companion layout joined into a spread either before or after the associated panelcode layout, using `ibefore` or `iafter`. These inserted pages optionally take a separate label, `ilabel`.

```panelcode
  1_1+r2,1 {: img='images/174.png' ibefore }
; 1_1+r2,1 {: img='images/174.png' iafter }
  {::: comicbook-us small }
```

Finally, a collection such as a spread or gallery can all be styled in the same way using a single argument. Here is a gallery:

-  styled with `iafter`:

```panelcode
  1_1+r2,1      {: img='images/174.png' }
; 2+c2_1+r2,1_1 {: img='images/175.png' }
; 1+c2_c6+c4_2  {: img='images/176.png' }
  {::: comicbook-us thumb iafter }
```

-  styled with `iover ishow`:

```panelcode
  1_1+r2,1      {: img='images/174.png' }
; 2+c2_1+r2,1_1 {: img='images/175.png' }
; 1+c2_c6+c4_2  {: img='images/176.png' }
  {::: comicbook-us thumb iover ishow }
```


##  Sizes

```panelcode

  1_2_3  {: default }
; 1_2_3  {: small }
; 1_2_3  {: thumb }
; 1_2_3  {: mini }
; 1_2_3  {: micro }
; 1_2_3  {: micro2 }

```

Panelcode can be rendered in different display sizes and styles for different purposes. Shown here: full size (default), thumb, mini, micro, and micro2.

These glyphs are the same html or SVG output, but rendered with a different CSS3 class label. The level of display detail is styled as well -- "thumb" has a caption and panel id, "mini" has only a caption, and "micro" is just a bare glyph image. Collections of Panelcode glyphs can be restyled at the gallery webpage level by changing a single class tag.

#### default style

```panelcode
  x
; 1
; 1_2
; 2_2
; 1_2_3
; 3_3_3
  {::: default }
```

#### small

```panelcode
  x
; 1
; 1_2
; 2_2
; 1_2_3
; 3_3_3
  {::: small }
```

#### thumb

```panelcode
  x
; 1
; 1_2
; 2_2
; 1_2_3
; 3_3_3
  {::: thumb }
```

#### mini

```panelcode
  x
; 1
; 1_2
; 2_2
; 1_2_3
; 3_3_3
  {::: mini }
```

#### micro

```panelcode
  x
; 1
; 1_2
; 2_2
; 1_2_3
; 3_3_3
  {::: micro }
```


## Spreads

```panelcode
  x
| 1
| 1_2
| 2_2
{:: thumb }

; x
| 1
| 1_2
| 2_2
{:: mini }

; 1_2_3
| 3_3_3
{:: thumb }

; 1_2_3
| 3_3_3
{:: mini }
```


## Sequences

The following sequence counts through the number of simple, row-based compositions of 1-3 rows, and 1-3 panels per row, giving a maximum grid of 3x3.

```panelcode
  0      {: label='0'     }
; 1      {: label='1'     }
; 2      {: label='2'     }
; 3      {: label='3'     }
; 1_1    {: label='1_1'   }
; 1_2    {: label='1_2'   }
; 1_3    {: label='1_3'   }
; 2_1    {: label='2_1'   }
; 2_2    {: label='2_2'   }
; 2_3    {: label='2_3'   }
; 3_1    {: label='3_1'   }
; 3_2    {: label='3_2'   }
; 3_3    {: label='3_3'   }
; 1_1_1  {: label='1_1_1' }
; 1_1_2  {: label='1_1_2' }
; 1_1_3  {: label='1_1_3' }
; 1_2_1  {: label='1_2_1' }
; 1_2_2  {: label='1_2_2' }
; 1_2_3  {: label='1_2_3' }
; 1_3_1  {: label='1_3_1' }
; 1_3_2  {: label='1_3_2' }
; 1_3_3  {: label='1_3_3' }
; 2_1_1  {: label='2_1_1' }
; 2_1_2  {: label='2_1_2' }
; 2_1_3  {: label='2_1_3' }
; 2_2_1  {: label='2_2_1' }
; 2_2_2  {: label='2_2_2' }
; 2_2_3  {: label='2_2_3' }
; 2_3_1  {: label='2_3_1' }
; 2_3_2  {: label='2_3_2' }
; 2_3_3  {: label='2_3_3' }
; 3_1_1  {: label='3_1_1' }
; 3_1_2  {: label='3_1_2' }
; 3_1_3  {: label='3_1_3' }
; 3_2_1  {: label='3_2_1' }
; 3_2_2  {: label='3_2_2' }
; 3_2_3  {: label='3_2_3' }
; 3_3_1  {: label='3_3_1' }
; 3_3_2  {: label='3_3_2' }
; 3_3_3  {: label='3_3_3' }
  {::: thumb}
```

An interesting property of Panelcode strings is that, once reduced to simple rows, each string is part of a numeric sequence, counting through the space from a blank page up to the maximum compositional grid. For example, given a work which is never more than 3 columns wide or 3 rows tall, any Panelcode is a ternary number (base 3), running from 1 (a full page) to `3_3_3` (a 3x3 grid).

Panelcodes (and hence the layouts the represent) can be sorted ), and these sortings have some interesting properties. and a set of strings can be sorted. This sorts a . ternary (base 3) number. complexity of each page. A zero-padded numeric sort (shown here) (those composed of simple rows) is that they can be sorted within a maximum grid (e.g. 2x2, 3x3, 4x4). Different sortings methods present different groupings of intuitively "similar" layouts. For example, an alphabetic sort groups layouts by increasing complexity of the top row (`1`, `2`, `3` panels) and then by the second row within each group (`1_1`, `1_2`, `1_3`), et cetera.


## Spaces

Consider the example of comics. While some genres of comics (such as newspaper strips) generally subdivide the available space into panels, many other forms commonly use compositions that leave significant regions of blank space.

```panelcode
  0_1  {: label='0_1' }
| 1_c2+0_1+0c2_0  {: label='1_c2+0_1+0c2_0' }
```

When a 0 is used to encode a blank region the corresponding empty area is rendered on the Panelcode grid. For example, 1_0 encodes a page whose bottom half is empty.

In the most complex cases, empty panels can also take column and row arguments, so compositions with large whitespace regions or complex combinations of blank and filled areas can be quickly marked: `(0+0c2r2,1)_(c2+0)_(1+c2)`.

```panelcode
  0                        {: label='0' }
; 0+1                      {: label='0+1' }
; 0+0+1                    {: label='0+0+1' }
; 0_1                      {: label='0_1' }
; 1_(0+1)                  {: label='1_(0+1)' }
; 0_(0+2)                  {: label='0_(0+2)' }
; (1+0)_1                  {: label='(1+0)_1' }
; (1+0)_(0+1)              {: label='(1+0)_(0+1)' }
; (1+0)_(1+0+1)            {: label='(1+0)_(1+0+1)' }
; (2+0)_1                  {: label='(2+0)_1' }
; (2+0)_(0+1)              {: label='(2+0)_(0+1)' }
; (1+0+0)_(2+0)            {: label='(1+0+0)_(2+0)' }
; 0_1_0                    {: label='0_1_0' }
; 0_1_2                    {: label='0_1_2' }
; 1_0_(1+0+1)              {: label='1_0_(1+0+1)' }
; 1_2_0                    {: label='1_2_0' }
; 0_(0+1)_2                {: label='0_(0+1)_2' }
; 1_(1+0)_(1+0+0)          {: label='1_(1+0)_(1+0+0)' }
; 0_(0+1+0)_0              {: label='0_(0+1+0)_0' }
; 1_3_(0+1)                {: label='1_3_(0+1)' }
; 1_(1+0+1)_3              {: label='1_(1+0+1)_3' }
; (1+0)_1_0                {: label='(1+0)_1_0' }
; (1+0)_1_(1+0)            {: label='(1+0)_1_(1+0)' }
; (0+1)_0_(0+2)            {: label='(0+1)_0_(0+2)' }
; (1+0)_(0+1)_1            {: label='(1+0)_(0+1)_1' }
; (1+0)_(1+0)_(0+1)        {: label='(1+0)_(1+0)_(0+1)' }
; 2_2_(0+2)                {: label='2_2_(0+2)' }
; (1+0)_3_1                {: label='(1+0)_3_1' }
; (1+0)_(2+0)_(1+0)        {: label='(1+0)_(2+0)_(1+0)' }
; 2_(0+2)_(0+0+1)          {: label='2_(0+2)_(0+0+1)' }
; (1+0+1)_1_1              {: label='(1+0+1)_1_1' }
; (2+0)_1_(1+0)            {: label='(2+0)_1_(1+0)' }
; 3_0_3                    {: label='3_0_3' }
; 3_(1+0)_0                {: label='3_(1+0)_0' }
; 3_(0+1)_(1+0)            {: label='3_(0+1)_(1+0)' }
; (1+0+0)_(1+0)_3          {: label='(1+0+0)_(1+0)_3' }
; 3_(0+1+0)_1              {: label='3_(0+1+0)_1' }
; (0+1+0)_(0+1+0)_2        {: label='(0+1+0)_(0+1+0)_2' }
; (1+0+1)_(0+1+0)_(1+0+1)  {: label='(1+0+1)_(0+1+0)_(1+0+1)' }
  {::: thumb }
```


## Ambiguity and Complexity

An empty or blank panel is a placeholder -- the `0` in a blank panel indicates that it does not count towards the total of a group or a total composition. By contrast, unencoded regions may count for one panel -- or more than one panel.

```panelcode
  1+u
; u2 + u4.r2 + 0, 0 + 1
```

An unencoded region is a collection of panel-like objects whose geometry is too complex or too non-cartesian to be encode in Panelcode shorthand.


## Shapes

```panelcode
  1 + 1.scale-50 _ 1.scale-50 + 1
; 3 _ 1 + 1.scale-200 + 1 _ 3
; 1.scale-90 + 1.scale-80 + 1.scale-70
_ 1.scale-60 + 1.scale-50 + 1.scale-40
_ 1.scale-30 + 1.scale-20 + 1.scale-10
```

```panelcode
  2_2 {: skew-fwd }
| 3_3_3 {: skew-back }
; 1+c2,c2+1 {: tilt-up }
| 3_3_3 {: tilt-down }
; 3{tilt-up}_3{tilt-down}
| 1+c2{tilt-up}_3_c2+1{tilt-up}
```

```panelcode
  2_2 {: rot-up }
| 3_3_3 {: rot-down }
```

```panelcode
  2_2
| 3_3_3
  {::: circle }
```

```panelcode
  0 _ 1.rot-up + 1 + 1.rot-down _ 0 {: label='1.rot-up; 1; 1.rot-down'}
; 1.rot-up-5 + 1.rot-up-10 + 1.rot-up-15
_ 1.rot-up-20 + 1.rot-up-25 + 1.rot-up-30
_ 1.rot-up-35 + 1.rot-up-40 + 1.rot-up-45
  {: label='1.rot-up-5  to  1.rot-up-45' }
; 1.rot-up-50 + 1.rot-up-55 + 1.rot-up-60
_ 1.rot-up-65 + 1.rot-up-70 + 1.rot-up-75
_ 1.rot-up-80 + 1.rot-up-85 + 1.rot-up-90
  {: label='1.rot-up-50 to  1.rot-up-90' }

; 1.rot-down-5 + 1.rot-down-10 + 1.rot-down-15
_ 1.rot-down-20 + 1.rot-down-25 + 1.rot-down-30
_ 1.rot-down-35 + 1.rot-down-40 + 1.rot-down-45
  {: label='1.rot-down-5  to  1.rot-down-45' }
; 1.rot-down-50 + 1.rot-down-55 + 1.rot-down-60
_ 1.rot-down-65 + 1.rot-down-70 + 1.rot-down-75
_ 1.rot-down-80 + 1.rot-down-85 + 1.rot-down-90
  {: label='1.rot-down-50 to  1.rot-down-90' }
```

Scale is a descriptions independent of the encoding direction. Whether using left-to-right (LTR) or right-to-left (RTL) encoding, a scaled panel will look the same.

This is not the case for skew, tilt, or rotation, however. These are described in terms of the reading / encoding order. Skewing "forward" leans in to the reading direction, "back" leans away. Tilting and rotate "up" rises in the reading direction, "down" falls.

```panelcode
  2_2    {: skew-fwd }
| 3_3_3  {: skew-back }
| 2+r2.skew-back
, c2.skew-back
_ skew-back + skew-fwd + skew-back
  {::: small}
```

```panelcode
  2_2    {: skew-fwd }
| 3_3_3  {: skew-back }
| 2+r2.skew-back
, c2.skew-back
_ skew-back + skew-fwd + skew-back
  {::: small prtl }
```


## Fill: color and style

```panelcode
  red + green + blue
_ yellow + magenta + cyan
```

```panelcode
  gray0 + gray1 + gray2 + gray3
_ gray4 + gray5 + gray6 + gray7
_ gray8 + gray9 + grayA + grayB
_ grayC + grayD + grayE + grayF
```

```panelcode
  texture1 + texture2
_ texture3 + texture4
```

```panelcode
  red + 1_2_red
; 1+red+1_1+2red_1
; 1+red_1+red
; red
; 2_1+red+1_2
; red+2_2_1+red
  {::: thumb }
```

```panelcode
  r2+1,1 | 1+r2,1 {:: black }
; r2+1,1 | 1+r2,1 {: black }
; (3,3){black}_2_1
  {::: thumb }
```

```panelcode
  1+c2_c2+1{:black}
| 1+c2_c2+black
| 1+c2_c2+black{:black}
  {::: small }
```


## Flow

### Comic Books


```panelcode
  1_1_1
; 1_2_2
; 2_2_3
; 1+c2_c2+1_3
; 3_3_3
  {::: comicbook-us small }
```


### Newspaper comicstrips: ratios and flow

The newspaper comic strip, as a form, has a very interesting relationship to panel geometry. Panels almost invariably hhave a fixed height and often have a fixed sequence of width ratios -- traditionally 3:1:2:2:1:3. These ratios were designed with a fluid layout property: a single piece is produced as a six-panel sequence may be laid out (or may have been laid out) in a variety of column counts and aspect ratios in different newspapers, taking up a quarter page or half page, respectively -- or a third of a page, if the optional first two panels are dropped -- or even (rarely) a full page.

When there is no specific page (or there are many pages, or the page is fluid) then Panelcode can render a sequence of panel geometry without a particular page formatting.

(NOTE: did this with w12, could create wx and/or use flex. Also overrode unit square dimensions manually-- could make horizontal scroll a style tmeplate).

```panelcode
  c3+1+c2+c2+1+c3 {: comicstrip-linear }

; c3.red
+ 1.yellow
+ c2.green
+ c2.cyan
+ 1.blue
+ c3.magenta {: w12 comicstrip-linear label='6 newspaper panels: (c3+1+c2+c2+1+c3)' }

```

The Panelcode here is a simple list of column ratios. It has been further annotated with color so that we can more easily see the panels flow between different layouts.

	(1.c3     + 1        + 1.c2       + 1.c2      + 1      + 1.c3)
	(1.c3.red + 1.yellow + 1.c2.green + 1.c2.cyan + 1.blue + 1.c3.magenta)

While a comic strip would never appear in this extreme horizontal format in a newspaper, this stream of panels can automatically flow into a dynamic layout -- for example with no particular columns specified, the CSS3 layout algorithm packs the panels into a unit square in a way that echos the row arrangement of the "Sunday full-page," one of the layouts for which the six-panel comic strip format was originally designed.

```panelcode
  c3.red
+ 1.yellow
+ c2.green
+ c2.cyan
+ 1.blue
+ c3.magenta {: w3 label='Unit square: c3+1+c2+c2+1+c3' }

; c3.red + 1.yellow + c2.green + c2.cyan + 1.blue + c3.magenta
  {: w3 comicstrip-sunday-full label='Sunday full: 4 cols' small}
  {:: default }
```

 Other major reconfigurations of the Sunday newspaper also emerge automatically from setting the number of layout columns, including the quarter-page, half-page, and full-page.

```panelcode
  c3.red + 1.yellow + c2.green + c2.cyan + 1.blue + c3.magenta
  {: w6 comicstrip-sunday-quarter label='Sunday quarter: 6 cols' }

; c2.green + c2.cyan + 1.blue + c3.magenta
  {: w4 comicstrip-sunday-third label='Sunday third: 4 cols' }

; c3.red + 1.yellow + c2.green + c2.cyan + 1.blue + c3.magenta
  {: w4 comicstrip-sunday-half label='Sunday half: 4 cols' }

; c3.red + 1.yellow + c2.green + c2.cyan + 1.blue + c3.magenta
  {: w3 comicstrip-sunday-full label='Sunday full: 4 cols' }

  {::: small }
```

One of the interesting properties of Panelcode is that these simple layout regroupings -- the kind that a newspaper might perform during layout -- are manifest in the transformation of one layout to another. Beginning with the Panelcode for a long strip of panels, we can indicate the row groups of any of the standard newspaper layouts merely by rearranging parantheses:

		no col, 1 rows:  (c3 + 1 + c2 + c2 + 1 + c3)
		 6 col, 2 rows:  (c3 + 1 + c2)_(c2 + 1 + c3)
		 4 col, 3 rows:  (c3 + 1)_(c2 + c2)_(1 + c3)
		 3 col, 4 rows:  (c3)_(1 + c2)_(c2 + 1)_(c3)

While in graphic novels and comicbooks the unit of composition is often the page itself, in the comicstrip the unit of composition is often part of a page -- whether in a newspaper publication or in a compendium of reprints. Panelcode can be used to mark up compositions of multiple layouts, layouts can be joined together into pages, and pages can be joined together into two-page spreads (or more).

**Examples**


**Links**: Daily strips might be in a single column or in a double-column -- e.g. see THE DAILY GRUNT, 2003 or a 1949 comics page from the Huntingdon Daily News. See also a Daily Herald Sunday page.


##  Direction

An encoding may indicate and be rendered in Lef-to-Right (LTR) or Right-to-Left (RTL) reading order.

```panelcode
  (r2+2,2)_(1+c2+c4) {: ltr }
| (r2+2,2)_(1+c2+c4) {: prtl }
```

Here the same Panelcode string is rendered in Japenese manga reading order on the right and as a mirrored translation on the left. Panel labels are preserved, with "1" starting in the upper right or upper left, respectively.

```panelcode
  r2+1,1   {: ltr }
| 2_1      {: ltr }
| 1+0_0+1  {: ltr }

; r2+1,1   {: prtl }
| 2_1      {: prtl }
| 1+0_0+1  {: prtl }

; r2+1,1
| 2_1
| 1+0_0+1
{:: prtl}

{::: small }
```

A sequence of pages can also be rendered RTL, as when encoding the pages of a tankoban. Here the Panelcode:

	    (r2+1,1)
	    2_1
	    1+0_0+1

...is rendered as a right-to-left page sequence of RTL pages, with panel "1" beginning on the far right and "9" ending on the lower left.

----------

----------


## Bleeds

A bleed is an annotation on a panel indicating that it proceeds over the margin and to the edge of the layout space. Panels bleeds are specified with the keyword `bleed` plus a directional suffix -- they may bleed up, down, back, or forward.


```panelcode
  bleed-up _ bleed-down
; bleed-back + bleed-fwd
  {::: small }
```


Direction can be combined. A panel with adjascent bleeds in two directions  proceeds to a corner. A panel with opposing bleeds cuts across the page in a band.


```panelcode
  bleed-up-back
+ bleed-up-fwd
_ bleed-down-back
+ bleed-down-fwd
; 0 + bleed-up-down  + 0
; 0 _ bleed-back-fwd _ 0
  {::: small }
```


Panels may also bleed to three different edges, for example `bleed-back-up-down`. For convenience, these bleeds may also be specified with the primary direction plus `-half`:


```panelcode
  bleed-back-half + bleed-fwd-half
; bleed-up-half _ bleed-down-half
  {::: small}
```


A full-page spread may also be full-bleed, with bleeds in all four directions. This may be written `bleed-up-down-back-fwd`, but for convenience it call simply be written `bleed-all`:


```panelcode
  1         {: label='no bleeds' }
| bleed-all {: label='bleed-all' }
  {::: small}
```


For quick writing and editing, bleed annotations may be minified with `b-` plus a string with one letter per directon. `-half` and `-all` are shortened to `h` and `a`.


```panelcode
  b-ub + b-u + b-uf
_ b-b  + 1   + b-f
_ b-db + b-d + b-df

; b-uh
_ b-bf
_ b-dh

; b-bh + b-ud + b-fh

; b-a
  {::: thumb }
```


Although it is rare, a group, row, or layout may also be assigned a bleed that applies to every panel inside it -- for example, rows of panels that all bleed to the top or bottom, or columns of panels that all bleed to the inside or outside of a spread.


```panelcode
  3 { b-u }
_ 3 { b-d }

; 1_1_1 {: b-f }
| 1_1_1 {: b-b }

; 1_1_1 {: b-b }
| 1_1_1 {: b-f }

  {::: thumb }
```

`-fwd` and `-back` here relate to the reading direction -- in a left-to-right us comic, `-fwd` bleeds to the right. In right-to-left panelcode marked `rtl` Japanese manga, `fwd` bleeds to the left. Because bleeds relate to the overall flow of the layout, the same encoding can be tagged for either `ltr` or `rtl` reading direction and it will render correctly with no other changes.


```panelcode
  c2.bleed-back + bleed-up-fwd
_ bleed-down-back + c2.bleed-down

; c2.bleed-back + bleed-up-fwd
_ bleed-down-back + c2.bleed-down
{: prtl label='rtl'}

  {::: small }
```


A layout may contain any combination of panels with bleeds and non-bleeds, in any directions. The overall composition is preserved when tagged `rtl`.


```panelcode
  r2.b-ub
+ 2, 1 + b-f
_ b-d
;  r2.bleed-up-back
+ 2, 1 + bleed-fwd
_ bleed-down
  {: prtl }
  {::: small}
```


In the CSS renderer bleeds directed towards the interior panels may be used to emulate panel overlaps -- but this is not what they are designed for.


```panelcode
  1 _ 2 + b-u
```


## Levels of specificity

```panelcode
  4 {: ilabel='4' }
| 1 _ 3 {: ilabel='1 _ 3' }
| 1 _ 1 + r2 , 1 {: ilabel='1 _ 1 + r2 , 1' }
| 1.c10.r6 , 1.c6.r6 + 1.c4.r10 , 1.c6.r4 {: ilabel='1.c10.r6 , 1.c6.r6 + 1.c4.r10 , 1.c6.r4 '}
  {::: comicbook-us small}
```


## Whitespace: delimited data entry

Because panelcode ignors linebreaks and whitespace , panelcode can be arranged in a variety of ways to make it more compact and minified, or to make writing , reading , or editing easier.

So this two-page spread can be described in a single line:

```panelcode
1_r2+1,1|c10.r6,c4.r10+c6.r8,c6.r2{:::small}
```
...or broken up in various ways, for example organizing each page of a spread on its own line with optional arguments immediately below.

```panelcode
1_r2+1,1
{:red}
|
c10.r6,c4.r10+c6.r8,c6.r2
{:blue}
  {:::small}
```

Panelcode can also be written in a delimiter-prefix style , with one code unit or set of arguments per line. Each line begins with a delimiter indicating that it is a new spread "`|`", layout "`;`" row "`_`", rowgroup line "`,`", or column "`+`".

```panelcode
  1
_ r2
+ 1
, 1
| c10.r6
, c4.r10
+ c6.r8
, c6.r2
  {:::small}
```

#### MISC


		| 3
		 _ 3
		 _ 1
		
		| 2
		+ c2
		 _ 1
		+ r2
		, 1
		 _ 1
		
		| 2
		+ c2
		 _ ( r2
		+ r3
		, 1
		  )
		 _ 1
		{: img='images/175.png' }
		{::: comicbook-us small}


```panelcode
| 3
 _ 3
 _ 1

| 2
+ c2
 _ 1
+ r2
, 1
 _ 1

| 2
+ c2
 _ ( r2
+ r3
, 1
  )
 _ 1
{: img='images/175.png' }
  {::: comicbook-us small}
```

----------

_&copy; 2017 Jeremy Douglass_