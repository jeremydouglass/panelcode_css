
```panelcode
r3.gray9 + c2.r2.gray9, 1 + 1z
{::: small }
```

# Panelcode: layout markup and visualization

_Jeremy Douglass_
_2017-12_

```panelcode
1; 2; 3
{::: mini }
```

**Panelcode** is a minimal markup language for describing visual compositions as abstract layouts.

Use Panelcode to **quickly encode visual layouts for search and visual exploration**. Panelcode is optimized to be quickly and easily writable, readable, and editable by hand, with an emphasis on speed, concision, and human judgement. The tools were developed with a particular focus on compositions made of panels such as those occuring in comics, graphic novels, newspapers, magazines, websites, et cetera, and the first test cases have been performed on comics.

Panelcode supports multiple encoding schemes, but examples in this document use its base scheme: gridcode. Panelcode also has many extensions and shorthands, a number of which are touched on here: markup for blank spaces, annotations, unencoded regions, panel shapes, and bleeds. Finally, Panelcode has renderers to create multiple outputs, including SVG (Scalable Vector Graphics), scaffolding TEI-XML, and HTML-Tables.

**This document focuses on the gridcode HTML5-CSS3 renderer**, which renders panelcode as either HTML5 web content or stand-alone SVG image files. It is an illustrated tour of some of Panelcode's rendering features. Rather than walking through the complete language syntax, or discussing the design of the parser, it instead focuses on the gridcode renderer and walks through an illustrated tour of basic examples of Panelcode and what you can do with it: describe, summarize, compare, annotate, and vizualize layouts. This renderer supports:

-  basic rows and columns
-  spans of multiple row / columns
-  blank spaces
-  marking multi-panel regions
-  panel shapes:
   -  skew
   -  scaling
   -  rotation
   -  scaling
-  panel annotations:
   -  color
   -  texture
   -  gutter color
-  sizes and styling:
   -  large, small, and thumb sizes
   -  mini and micro glyphs
-  media formats:
   -  abstract
   -  comicbooks
   -  newspaper comicstrips
-  right-to-left rendering


## Basics: Rows and Columns

```panelcode
  1
; 2 | 3; 1_1 | 1_1_1
; 1_2 | 1_2_3 | 4_3_2_1 | 6_4_3_5_2_1
; 2_2 | 3_3_3
{::: mini }
```

```panelcode
r3.gray9 + c2.r2.gray9, 1 + 1z
{::: mini }
```

Panelcode is a very efficient shorthand when encoding simple page layouts. Encodings may be written by hand and then parsed and programmatically processed for many purposes: searching, analyzing, rendering graphics and creating information visualizations, or producing scaffolds for layout editing or marking up pages with metadata.

A number in Panelcode signifies a simple row of panels. A single number ("`3`") is a valid Panelcode string that describes a layout with a single row of 3-columns.

```panelcode
  1 {: label='1'}
; 2 {: label='2'}
| 3 {: label='3'}
{::: small }
```

The Panelcode strings "`1`" "`2`" and "`3`" appearing at the bottom of each glyph are not page numbers -- each number describes the row of panels in each layout.

```panelcode
  4
{::: comicstrip-daily mini dropcap }
```

In Panelcode, a sequence of panels is the basic unit of description: A "`4`" means four panels, as in a typical daily newspaper comic, for example. Those panels proceed in whatever typical reading order they were encoded in -- left-to-right rows and top-to-bottom columns, in this case, but Panelcode input and/or output can be automatically mirrored for e.g. manga, scanlations and imported reprints.

```panelcode
  1_1   {: label='1_1'}
| 1_1_1 {: label='1_1_1'}
{::: small }
```

Rows are joined into layout stacks using underscores: `1_1` or `1_1_1`.

```panelcode
  3     {: label='3'}
| 1_1_1 {: label='1_1_1'}
{::: mini dropcap nolabel }
```

The default row operator "`_`" (the underscore) is the glue used to build complex pages by connecting of simple rows. While `3` described three horizontal panels, `1_1_1` describes three vertical panels. This can be made more concise with one of the shorthand extensions, but it reflects a fundamental aspect of gridcode encoding design: the primary reading order (in this case, left-to-right) commonly appears in a different way than the secondary reading order (top-to-bottom). In fact, panels connected by the column operator "`+`" are commonly collapsed into row counts: the horizontal panelcode layout `3 could also be writtn `1+1+1``; the vertical panelcode layout `1_1_1` can (with an extension) be written 3v.

```panelcode
  1_2         {: label='1_2'}
| 1_2_2       {: label='1_2_3'}
| 4_3_2_1     {: label='4_3_2_1'}
| 6_5_4_3_2_1 {: label='6_5_4_3_2_1'}
{::: small }
```

Combine different row counts into one layout: `1_2_3`

```panelcode
  1_2_3     {: label='1_2_3'}
{::: mini dropcap }
```

When the row operator is used to combine rows then each row may have different panel counts, and every row is assumed by default to be full-width with its own unique panel dimensions.	This is fundamentally unlike a spreadsheet, HTML &lt;table&gt;, or other tabular data presentation system. In	such systems columns are fixed, and rows must be marked of in units or multiples of those columns. In order to create a `1_2_3_4` on a spreadsheet requires 12 columns, and each row must be expressed in multiples of those columns.

These fixed column constraints are essentially the limitations of attempting to express design grids in tabular data presentation formats. By contrast Panelcode is based on a flexible design grid paradigm -- and in fact uses the emerging CSS flex / flexbox and CSS grid standards for this renderer, which are likewise based on an evolution of web design away from tabular data and towards design grids.

> [Although note that the original Panelcode HTML-Tables renderer accomplished this by making every row or rowgroup its own independent table -- such workarounds are often painful, but not impossible.]

```panelcode
  2_2         {: label='2_2'}
| 3_3_3       {: label='3_3_3'}
{::: small }
```

Combine 2_2 or 3_3_3 row counts into stacks to form grids.

```panelcode
  8_8_8_8_8_8_8_8 {: label='8_8_8_8_8_8_8_8'}
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
  c3.grayC + 1.grayB + c2.grayA + c2.gray9 + 1.gray8 + c3.gray7
  {: w4 label='Abstract unit square (1:1)' }
; c3.grayC + 1.grayB + c2.grayA + c2.gray9 + 1.gray8 + c3.gray7
  {: w4 comicstrip-sunday-half label='Newspaper comicstrip ("Sunday half" aspect)'}
; c3.grayC + 1.grayB + c2.grayA + c2.gray9 + 1.gray8 + c3.gray7
  {: w4 comicbook-us label='Comicbook page (US aspect)' }

```

The glyph for `c3+1_2_1+c3` rendered on a unit square, then rerendered while tagged as a newspaper comicstrip (14x10 aspect ratio, Sunday half-page) or as a comicbook page (11x17 aspect ratio, contemporary US comic). Panelcode output may visually match the specific media it was encoded from, or it may also abstract page dimensions when comparing layouts in mixed media collections.


## Images and Panelcode

```panelcode
  1_1+r2,1 {: img='images/174.png'}
; 1_1+r2,1 {: img='images/174.png' iover }
; 1_1+r2,1 {: img='images/174.png' ibefore }
; 1_1+r2,1 {: img='images/174.png' iafter }
{::: comicbook-us mini }
```

Panelcode can also specify methods for companion image display. Specfing an img path displays that image on top of the layout. The ishow and ihide attributes make the image interactively show or hide on hover.

```panelcode
  1_1+r2,1 {: img='images/174.png'}
; 1_1+r2,1 {: img='images/174.png' ishow }
; 1_1+r2,1 {: img='images/174.png' ihide }
{::: comicbook-us small }
```

Use `iover` to style the image as semi-opaque (onion skin). The “over” image layer can also be fully shown or fully hidden with `ishow` or `ihide`.

```panelcode
  1_1+r2,1 {: img='images/174.png' iover }
; 1_1+r2,1 {: img='images/174.png' iover ishow }
; 1_1+r2,1 {: img='images/174.png' iover ihide }
{::: comicbook-us small }
```

Images can also be displayed in a companion layout joined into a spread either before or after the associated panelcode layout, using `ibefore` or `iafter`. These pages optionally take a separate label, `ilabel`.

```panelcode
  1_1+r2,1 {: img='images/174.png' ibefore }
; 1_1+r2,1 {: img='images/174.png' iafter }
{::: comicbook-us small }
```

Finally, a collection such as a spread or gallery can all be styled in the same way using a single argument. Here is a gallery:

-  styled with `iafter`:

```panelcode
  1_1+r2,1 {: img='images/174.png' }
; 2+c2_1+r2,1_1 {: img='images/175.png' }
; 1+c2_c6+c4_2 {: img='images/176.png' }
{::: comicbook-us thumb iafter }
```

-  styled with `iover ishow`:

```panelcode
  1_1+r2,1 {: img='images/174.png' }
; 2+c2_1+r2,1_1 {: img='images/175.png' }
; 1+c2_c6+c4_2 {: img='images/176.png' }
{::: comicbook-us thumb iover ishow }
```


## Spans

```panelcode
  2
; 1+c2 | c2 + 1
; 2_2
; 1+c2 _ c2+1 | c2+1 _ 1+c2
{::: mini }
```

```panelcode
  r2+1,1 | 1+r2,1
; 1_2 | 2_1
; 1_2 | 2_1 | r2+1,1 | 1+r2,1
; c2.r2+1, 1
{::: mini }
```

Spans are a simple way of creating more complex layouts than combining rows will allow. A span specifies that a panel is proportionately wider that other in its row (column span) or that a panel occupies multiple rows in a rowgroup (row span). A panel may have both its column span and row span specified. In Panelcode spans are indicated with the `c` and `r` attributes, which are attached to panels as suffixes.

### Column  Spans

```panelcode
  2          {: label='2' }
; 1+c2       {: label='1+c2' }
| c2+1       {: label='c2+1' }
; 2_2        {: label='2_2' }
; 1+c3_c3+1  {: label='1+c3_c3+1' }
| c3+1_1+c3  {: label='c2+1_1+c3' }
{::: small }
```

A row with one column twice as wide as the other may be written 1+c2. Why? Follow these steps:

	    1:    2
	    2:    2(1 + 1   )
	    3:    2(1 + 1.c2)
	    4:    2(1 + c2  )
	    5:     (1 + c2  )
	    6:      1 + c2

The panelcode `2` is expanded into a group `(1+1)` of individual panels. One of those panels is annotated with the suffix `.c2` -- this sets the column span equal to 2, making the second column twice as wide as the first (and its default of 1). The result is `2(1+1.c2)`

We can additionally note that almost all of this is optional. A columnspan modifies a single panel by default, so `1.c2` may simply be written `c2`. The `2(` outside the group is hint at the contents, but it is also optional -- and there is only one group, so the group markers are also optional. The concise result? `1+c2`.

### Row Spans

```panelcode
  r2+1,1  {: label='r2+1,1' }
| 1+r2,1  {: label='1+r2,1' }
{::: small }
```

For rows that cross cross vertically into multiple rows, a special rowspan indicator is used with "r" for row plus the number of rows to span ("r2", "r3").

This concept of a "rowspan" argument is present in early HTML-Tables, and also incorporated into CSS-Grid. Panelcode uses these concepts in part because they are longstanding traditions in rendering tabular data, and in part because shared concepts make it easier to parse a panecode string into a straightforward sequence of HTML or CSS elements that can directly render the described layout.

```panelcode
  1_2  {: label='1_2' }
| 2_1  {: label='2_1' }
{::: small }
```

Compare with these very similar panel groups. Because the long panel is oriented horizontally rather than vertically these layouts can be described with a long row ("`1`"), and the encoding does not require rowspan notation. In fact, these do not require special column notation either -- a full-row panel is simply "`1`".

```panelcode
  1_2     {: label='1_2' }
| 2_1     {: label='2_1' }
| r2+1,1  {: label='r2+1,1' }
| 1+r2,1  {: label='1+r2,1' }
{::: small }
```

These four C-shaped compositions of 3 panels -- two based on row spans, two not -- are uncommon in newspaper comics, but are some of the most common building blocks of comic book pages.

Column spans and row spans may also be combined, as in the multi-row, multi-column panel in this example:

```panelcode
  c2.r2+1,1  {: label='c2.r2+1,1' }
{::: small }
```


##  Display sizes and styles

```panelcode

  1_2_3 {: default label='default'}
; 1_2_3 {: small label='small'}
; 1_2_3 {: thumb label='thumb'}
; 1_2_3 {: mini label='mini'}
; 1_2_3 {: micro label='micro'}
; 1_2_3 {: micro2 label='micro2'}

```

Panelcode can be rendered in different display sizes and styles for different purposes. Shown here: full size (default), thumb, mini, micro, and micro2.

These glyphs are the same html or SVG output, but rendered with a different CSS3 class label. The level of display detail is styled as well -- "thumb" has a caption and panel id, "mini" has only a caption, and "micro" is just a bare glyph image. Collections of Panelcode glyphs can be restyled at the gallery webpage level by changing a single class tag.

#### default style

```panelcode
  x      {: label='x'}
; 1      {: label='1'}
; 1_2    {: label='2'}
; 2_2    {: label='2_2'}
; 1_2_3  {: label='1_2_3'}
; 3_3_3  {: label='3_3_3'}
{::: default }
```

#### small

```panelcode
  x      {: label='x'}
; 1      {: label='1'}
; 1_2    {: label='2'}
; 2_2    {: label='2_2'}
; 1_2_3  {: label='1_2_3'}
; 3_3_3  {: label='3_3_3'}
{::: small }
```

#### thumb

```panelcode
  x      {: label='x'}
; 1      {: label='1'}
; 1_2    {: label='2'}
; 2_2    {: label='2_2'}
; 1_2_3  {: label='1_2_3'}
; 3_3_3  {: label='3_3_3'}
{::: thumb }
```

#### mini

```panelcode
  x      {: label='x'}
; 1      {: label='1'}
; 1_2    {: label='2'}
; 2_2    {: label='2_2'}
; 1_2_3  {: label='1_2_3'}
; 3_3_3  {: label='3_3_3'}
{::: mini }
```

#### micro

```panelcode
  x      {: label='x'}
; 1      {: label='1'}
; 1_2    {: label='2'}
; 2_2    {: label='2_2'}
; 1_2_3  {: label='1_2_3'}
; 3_3_3  {: label='3_3_3'}
{::: micro }
```


## Spreads

```panelcode
  x      {: label='x'}
| 1      {: label='1'}
| 1_2    {: label='2'}
| 2_2    {: label='2_2'}
{:: thumb }

; x      {: label='x'}
| 1      {: label='1'}
| 1_2    {: label='2'}
| 2_2    {: label='2_2'}
{:: mini }

; 1_2_3  {: label='1_2_3'}
| 3_3_3  {: label='3_3_3'}
{:: thumb }

; 1_2_3  {: label='1_2_3'}
| 3_3_3  {: label='3_3_3'}
{:: mini }
```

## Sequences and Sorting

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

## Blanks

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

## Complexity and ambiguity

An empty or blank panel is a placeholder -- the `0` in a blank panel indicates that it does not count towards the total of a group or a total composition. By contrast, unencoded regions may count for one panel -- or more than one panel.

```panelcode
  1+u
; u2 + u4.r2 + 0, 0 + 1
```

An unencoded region is a collection of panel-like objects whose geometry is too complex or too non-cartesian to be encode in Panelcode shorthand.

## Shape annotations


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
{::: small rtl}
```

## Color Styles (panel marking)

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

## Comic Books

```panelcode
  1_1_1
; 1_2_2
; 2_2_3
; 1+c2_c2+1_3
; 3_3_3
{::: comicbook-us small }
```

## Newspaper comicstrips: ratios and flow

The newspaper comic strip, as a form, has a very interesting relationship to panel geometry. Panels almost invariably hhave a fixed height and often have a fixed sequence of width ratios -- traditionally 3:1:2:2:1:3. These ratios were designed with a fluid layout property: a single piece is produced as a six-panel sequence may be laid out (or may have been laid out) in a variety of column counts and aspect ratios in different newspapers, taking up a quarter page or half page, respectively -- or a third of a page, if the optional first two panels are dropped -- or even (rarely) a full page.

When there is no specific page (or there are many pages, or the page is fluid) then Panelcode can render a sequence of panel geometry without a particular page formatting.

(NOTE: did this with w12, could create wx and/or use flex. Also overrode unit square dimensions manually-- could make horizontal scroll a style tmeplate).

```panelcode
  c3+1+c2+c2+1+c3
  
; c3.red
+ 1.yellow
+ c2.green
+ c2.cyan
+ 1.blue
+ c3.magenta {: w12 label='6 newspaper panels: (c3+1+c2+c2+1+c3)' }

```

The Panelcode here is a simple list of column ratios. It has been further annotated with color so that we can more easily see the panels flow between different layouts.

	(1.c3     + 1        + 1.c2       + 1.c2      + 1      + 1.c3)
	(1.c3.red + 1.yellow + 1.c2.green + 1.c2.cyan + 1.blue + 1.c3.magenta)

While a comic strip would never appear in this extreme horizontal format in a newspaper, this stream of panels can automatically flow into a dynamic layout -- for example with no particular columns specified, the CSS3 layout algorithm packs the panels into a unit square like this:


```panelcode
  c3.red
+ 1.yellow
+ c2.green
+ c2.cyan
+ 1.blue
+ c3.magenta {: w3 label='6 newspaper panels: (c3+1+c2+c2+1+c3)' }
```

Not coincidentally, the CSS3 Grid layout algorithm has here recreated the "Sunday full-page," one of the layouts for which the six-panel comic strip format was originally designed. Other major reconfigurations of the Sunday newspaper also emerge automatically from setting the number of layout columns, including the quarter-page, half-page, and full-page.

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


##  Direction: LTR and RTL

An encoding may indicate and be rendered in LTR or RTL reading order.

```panelcode
  (r2+2,2)_(1+c2+c4){: ltr label='LTR: (r2+2,2)_(1+c2+c4)'}
| (r2+2,2)_(1+c2+c4){: rtl label='RTL: (r2+2,2)_(1+c2+c4)'}
```

Here the same Panelcode string is rendered in Japenese manga reading order on the right and as a mirrored translation on the left. Panel labels are preserved, with "1" starting in the upper right or upper left, respectively.

```panelcode
  r2+1,1   {: label='LTR: r2+1,1' }
| 2_1      {: label='LTR: 2_1' }
| 1+0_0+1  {: label='LTR: 1+0_0+1' }
{:: ltr small }

; r2+1,1   {: label='RTL: r2+1,1' }
| 2_1      {: label='RTL: 2_1' }
| 1+0_0+1  {: label='RTL: 1+0_0+1' }
{:: rtl small }
```

A sequence of pages can also be rendered RTL, as when encoding the pages of a tankoban. Here the Panelcode:

	    (r2+1,1)
	    2_1
	    1+0_0+1

...is rendered as a right-to-left page sequence of RTL pages, with panel "1" beginning on the far right and "9" ending on the lower left.

----------

----------


## Bleeds

Bleeds are an annotation on a panel indicating that it proceed to some edge(s) of the space. A panel may bleed to one edge, two edges, three edges, or four.

A panel may bleed in any direction or any combination of directions, including all directions (full page).

```panelcode
  bl.up _ 1 _ bl.down
; bl.left + 1 + bl.right
; 1.bl.up.left
+ 1.bl.up
+ 1.bl.up.right
_ 1.bl.left
+ 1
+ 1.bl.right
_ 1.bl.down.left
+ 1.bl.down
+ 1.bl.down.right
; bl.up.down.left.right
```
(Currently full-page "left" bleeds have a display bug)

Panel bleeds may go to any edge or to any corner (two edges).

(Currently "up" bleeds have a text alignment bug.)

(Currently "left" bleeds cover the center region of two page spreads.)

A layout may contain any combination of panels with bleeds and non-bleeds, in any directions.

```panelcode
  r2+2,2_1
; r2.bl.up.left
+ 2, 1 + bl.right
_ bl.down
```

```panelcode
  r2+2,2_1
; r2.bl.up.left
+ 2, 1 + bl.right
_ bl.down
{::: rtl}
```

A minor issue: bleeds on the right side in a 2-unit spread may cross the centerline.

Some issues with needing to be in a spread div otherwise the left edge loses containment. Some magic numbers in the css to manage offsets that go along with the scaling.

The label div needs to come after bleed panels in order to be above them (visible) -- could try controlling with z-height.

----------

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