## MISC

load a markdown file

1. stream stdin or load from file
2. specify input method
3. detect input method?

2. parse fenced code blocks
3. insert render as replace, or before, or after each block

4. optionally add controls and open/closed
5. optionally add global open/closed and size control
6. optionally pass global open/closed and size control arg
7. optionally add syntax highlighting (and controls?)
8. optionally write out panelcode-grid.css, jquery, etc. inline or as separate files.

9. stream output to stdout or save to file

markdown file (w options)

(stdin | load) detect? 

    # load md-fenced panelcode
    with open(mdpath, w) as md_file:
    
    pc_html_str = md_to_html(md_file, img=true, code=true, highlight=true)

    md_to_html():



## Scaffold

1. use jinja
2. for n in files
3. output a text file that is prepopulated with panelcode, img arguments, and default settings



## Size view selector:

css jquery dropdown to change panelcode display class

-  https://stackoverflow.com/questions/12631746/change-css-based-on-drop-down-selections-value-using-jquery
-  https://api.jquery.com/change/
-  include script cdn line
-  http://jsfiddle.net/rajaadil/LxkY3/2/
-  https://www.w3schools.com/css/css_dropdowns.asp
-  https://css-tricks.com/dropdown-default-styling/
-  https://www.w3schools.com/jquery/jquery_css_classes.asp
-  https://stackoverflow.com/questions/13531518/change-body-class-from-a-drop-down-list-using-jquery


https://stackoverflow.com/questions/13531518/change-body-class-from-a-drop-down-list-using-jquery:


        $(document).ready(function(){
          // On load
          $('#MyDropDownId').change(function(){ // on change event
            $('body')
            .removeClass() // remove the classes on the body
             // or removeClass('class1 class2 ...') in order to not affect others classes
            .addClass( $(this).val() ); // set the new class
          })
        })
        
        <select id="MyDropDownId">
          <option value="class1">First Class</option>
          <option value="class2">2nd Class</option>
        </select>


ex2:

        $("form h2").click(function() {
            var form = $(this).closest("form");
            $("#"+$(this).text().trim()).prop('checked', true);
            form.find('h2').removeClass('selected');
            $(this).addClass('selected');
        });

script inclusion:

    <script src="whatever.js" type="text/javascript"></script>

    <script type="text/javascript">
    </script>



<script type="text/javascript">
  $(document).ready(function(){
    // On load
    $('#pcsize').change(function(){ // on change event
      $('.gallery')
      .removeClass('default small thumb mini micro micro2'
      // remove the classes on the body
      // or removeClass('class1 class2 ...') in order to not affect others classes
      .addClass( $(this).val() ); // set the new class
    })
  })
</script>



 + 
Panelcode is a minimal markup language -- it can be quickly written and edited "by hand" for 

Panelcode can be parsed, searched, and transformed.

Panelcode, either "raw" or embedded in something (HTML, 
JSON, Markdown, YAML. XML) can be parsed, searched, and transformed. Examples of a







parse, then pass
 - with with helper script
 - with pyde app

panelcode parsing mode
-  by default (raw panelcode)
-  by flag
-  by file extension(s) auto-detection
   -  .panelcode.txt, .txt, other
   -  .md, .mmd., .markdown etc.
   -  .yml, .yaml etc.
   -  .csv, .tsv, .etsv? etc.
-  by content auto-detection
   -  isRaw
   -  isMarkdown
   -  isCSV
   -  isHTML ( ... php/asp/etc.?)
   -  isYAML
   -  isCBML
   -  isTEI
   -  isXML




1. preserve ## for markdown headings
    1. might as well support ```panelcode and ```panelcode* too
1. Do lines contain ``` or ```*?
    1. note the * if there is one
    1. if ``` is empty or ```panelcode or ```panelcode*
        1. slice out lines between match pair ()
        1. try to parse
    1. else return as-is



Could this all be done with pyparsing? I think so. Much better, actually, something like:

fence_open = '```' + Optional(alphanums)
fence_close = '```'
fenced-pcode = fence_open + root + fence_close


# or implement fenced_pcode as a nestedExpr()

doctext = 
fenced-pcode-doc_unit = doctext | fenced-pcode
fenced-pcode-document = OneOrMore(fenced-pcode-doc_unit)

 a ``` with an optional panelcode+*, then a root, then a ```.

A text-with-embedded-panelcode is any number of panelcode blocks and anything



Image IDs -- name=id

optional gen-img arg:
    <img "src=name.png"/>



preparse draft junk:




"""Panelcode preparse methods for detecting types of panelcode for parsing and
   cleaning, joining, segmenting or labeling them before parsing.
"""


# https://stackoverflow.com/questions/159118/how-do-i-match-any-character-across-multiple-lines-in-a-regular-expression
# https://ideone.com/A21CXy
import re
s = "abcde\n		fghij<FooBar>";
rx = r"(.*)<FooBar>";
m = re.search(rx, s, flags=re.S)
if m:
	print(m.group(1))
    
# pyparsing.py
#        if multiline:
#           self.flags = re.MULTILINE | re.DOTALL
#           self.pattern = r'%s(?:[^%s%s]' % \
#               ( re.escape(self.quoteChar),
#                 _escapeRegexRangeChars(self.endQuoteChar[0]),
#                 (escChar is not None and _escapeRegexRangeChars(escChar) or '') )



# https://www.regextester.com/65535
# ^`{3}([\S]+)?\n([\s\S]+)\n`{3}
# my revision:
# (\n[ ]{0,3}`{3,})([\S ]+)?\n([\s\S]+)(\1)\n


import pyparsing as pp

markdown_block = pp.Regex(r"[\n]*")
newcol         = pp.Literal("+")

fence_b_open  = pp.LineStart() + pp.Optional(pp.White()) + pp.Literal('```') + pp.Optional(pp.OneOrMore('`')) + pp.Optional(info_string) + pp.Optional(pp.White()) + pp.LineEnd()
fence_b_close = pp.LineStart() + pp.Optional(pp.White()) + pp.Literal('```') + pp.Optional(pp.OneOrMore('`')) + pp.Optional(pp.White()) + pp.LineEnd()
fence_t_open  = pp.LineStart() + pp.Optional(pp.White()) + pp.Literal('~~~') + pp.Optional(pp.OneOrMore('~')) + pp.Optional(info_string) + pp.Optional(pp.White()) + pp.LineEnd()
fence_t_close = pp.LineStart() + pp.Optional(pp.White()) + pp.Literal('~~~') + pp.Optional(pp.OneOrMore('~')) + pp.Optional(pp.White()) + pp.LineEnd()

fence_b_block = fence_b_open + content + fence_b_close
fence_t_block = fence_t_open + content + fence_t_close

 | fence_t_open + content + fence_t_close

code_block = pp.nestedExpr(opener='```', closer='```') | 


def decomment(item, delim):
    """Remove whole line and end-of-line comments marked with a delimiter."""
    for itemline in item:
        seg = itemline.split(delim, 1)[0].strip()
        if seg != '':
            yield seg
        else:
            ## align line numbers in oarse error checking with original
            yield ''

def fence_splitter(item, delim='```'):
    sections = []
    sectno = 0
    for itemline in item:
        if



# http://pygments.org/docs/lexerdevelopment/#using-multiple-lexers

from pygments.lexer import RegexLexer, bygroups
from pygments.token import *

class PanelcodeLexer(RegexLexer):
    name = 'Panelcode'
    aliases = ['panelcode', 'pc']
    filenames = ['*.panelcode', '*.pcode', '*.panelcode.txt', '*.pcode.txt']

    tokens = {
        'root': [
            (r'\s+', Text),
            (r'#.*?$', Comment),
            (r'{.*?}$', Keyword),
            (r'(.*?)(\s*)(=)(\s*)(.*?)$',
             bygroups(Name.Attribute, Text, Operator, Text, String))
        ]
    }





# http://mistune.readthedocs.io/en/latest/

import mistune
from pygments import highlight
from pygments.lexers import get_lexer_by_name
from pygments.formatters import html

class HighlightRenderer(mistune.Renderer):
    def block_code(self, code, lang):
        if not lang:
            return '\n<pre><code>%s</code></pre>\n' % \
                mistune.escape(code)
        lexer = get_lexer_by_name(lang, stripall=True)
        formatter = html.HtmlFormatter()
        return highlight(code, lexer, formatter)

renderer = HighlightRenderer()
markdown = mistune.Markdown(renderer=renderer)
print(markdown('```python\nassert 1 == 1\n```'))



-  **** https://github.github.com/gfm/#fenced-code-blocks - great spec with text cases 
-  simpler: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet

misc:
-  https://stackoverflow.com/questions/9361521/pyparsing-capturing-groups-of-arbitrary-text-with-given-headers-as-nested-lists
-  https://stackoverflow.com/questions/26600333/pyparsing-whitespace-match-issues



    # data_clean = preparse.decomment(data_list, '#')
    # data_str = "\n".join(data_clean)
    # data_str = ''.join(data_list)

    # data_fence_list = fences.split(data_str)
    # result_list = []
    # for idx, graph in enumerate(data_fence_list):
    #     if idx%5 == 0:
    #         result_list.append(str(graph))
    #     if idx%5 == 3:
    #         try:
    #             graph = "\n".join(preparse.decomment(graph, '#'))
    #             pcode_obj = parser.parse(graph, parser.root)
    #             html_lines = render.pobj_to_html5_ccs3_grid(pcode_obj)
    #             result_list.append(''.join(html_lines))
    #             # sys.stdout.write("\n".join(html_lines))
    #         except parser.pp.ParseException as err:
    #             result_list.append(str(graph))


    # data_clean = preparse.decomment(data_list, '#')


    # if '```' in data_str:
    #     graphs = data_str.split('```')
    #     for idx, graph in enumerate(graphs):
    #         if idx%2 == 1:
    #             try:
    #                 pcode_obj = parser.parse(graph, parser.root)
    #                 html_lines = render.pobj_to_html5_ccs3_grid(pcode_obj)
    #                 sys.stdout.write("\n".join(html_lines))
    #             except parser.pp.ParseException:
    #                 sys.stdout.write(graph)
    #         else:
    #             sys.stdout.write(graph)
    #     return

    # else:
    #     try:
    #         pcode_obj = parser.parse(data_str, parser.root)
    #         html_lines = render.pobj_to_html5_ccs3_grid(pcode_obj)
    #         for line in html_lines:
    #             sys.stdout.write(line)
    #         return
    #     except parser.pp.ParseException:
    #         pass
    # sys.stdout.write('none')
    # sys.stdout.write("\n".join(data_list))



decomment:

    ## if isinstance(item, str):
    ##     if '/n' in item:
    ##         item = item.split('/n')
    ##     else:
    ##         item = [item]



