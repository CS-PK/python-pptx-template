**Welcome to python-pptx-template's Documentation**

# IN DEV - This documentation is striaght up copied from docxtpl :) aim is to create something similar for ppts
**Quickstart**

Install using pip:

```bash
pip install python-pptx-template 
```

or using conda:

```bash
conda install python-pptx-template --channel conda-forge
```

**Usage:**

```python
from pptx_template import PptxTemplate

ppt = PptxTemplate("my_ppt_template.pptx")
context = { 'company_name': "World company" }
ppt.render(context)
ppt.save("generated_presentation.pptx")
```

**Introduction**

This package leverages two major packages:

*   **python-pptx** for reading, writing, and creating sub-presentations
*   **jinja2** for managing tags within the template pptx

**python-pptx-template** was created because while **python-pptx** is excellent for creating presentations, it is not ideal for modifying them.

The concept is to start by creating a sample presentation in Microsoft PowerPoint, incorporating the desired complexity—images, tables, footers, headers, variables, and other PowerPoint features. While editing, directly insert jinja2-like tags into the presentation and save it as a .pptx file (an XML-based format) to serve as your .pptx template.

Now, utilize **python-pptx-template** to generate numerous PowerPoint presentations from this template and associated context variables.

**Jinja2-like Syntax**

As with Jinja2, you can use all its tags and filters within the PowerPoint presentation. However, there are certain restrictions and extensions for proper functionality:

**Restrictions**

*   Regular jinja2 tags should only be used within the same run of a text box; they cannot span multiple text boxes, table cells, or runs.
*   To manage text boxes, table cells, and entire runs with their styles, use the special tag syntax explained in the next section.

**Extensions**

**Tags**

To control text boxes, table cells, and runs, use the following special syntax:

*   `{% textbox jinja2_tag %}` for text boxes
*   `{% tr jinja2_tag %}` for table rows
*   `{% tc jinja2_tag %}` for table columns
*   `{% r jinja2_tag %}` for runs

These tags instruct **python-pptx-template** to correctly place the actual jinja2 tags (without textbox, tr, tc, or r) in the presentation's XML source code. Additionally, they tell the package to **remove** the text box, table row, table column, or run where the tags are situated.

**Example:**

```
{% textbox if display_textbox %}
This text box will only appear if display_textbox is True
{% textbox endif %}
```

**Displaying Variables**

*   Use double braces `{{ <var> }}` for basic variable display.
*   If `<var>` is a string, `\n`, `\a`, `\t`, and `\f` will be translated into newlines, tabs, and page breaks, respectively.

**Comments**

Add jinja-like comments to your template:

```
{# textbox this is a comment in a text box #}
{# tr this is a comment in a table row #}
{# tc this is a comment in a table cell #}
```

**Splitting and Merging Text**

Merge a jinja2 tag with the previous or next line using `{%-` and `-%}`, respectively.

**Escaping Delimiters**

To display `{%, %}`, `{{`, or `}}`, use `{_%, %_}`, `{_{`, or `}_}`.

**Tables**

**Spanning**

Span table cells horizontally using the `colspan` tag:

```
{% colspan <var> %} 
```

`<var>` must contain an integer specifying the number of columns to span.

**Cell Color**

To change a table cell's background color, put the following tag at the cell's beginning:

```
{% cellbg <var> %} 
```

`<var>` must hold the color's hexadecimal code **without** the hash sign.

**Rich Text**

Use `{{r <var> }}` (note the 'r') and a RichText object within the `var` variable to dynamically add styling.

**Example:**

```python
from pptx_template import RichText

text = RichText("This is bold red text")
text.font.bold = True
text.font.color.rgb = RGBColor(255, 0, 0)
```

In your template:

```
{{r text}}
```

**Hyperlinks with RichText**

Add hyperlinks using RichText:

```python
from pptx_template import PptxTemplate, RichText

ppt = PptxTemplate('your_template.pptx')
rt = RichText('Click here for ')
rt.add('Google', url_id=ppt.build_url_id('http://google.com'))
```

In your template:

```
{{r rt}}
```

**Inline Images**

Dynamically add images to your presentation using `{{ <var> }}`, where `<var>` is an instance of `pptx_template.InlineImage`.

**Example:**

```python
my_image = InlineImage(ppt, 'path/to/image.jpg') 
```

**Sub-Presentations**

Template variables can hold complex sub-presentation objects built from scratch using python-pptx methods.

**Escaping**

By default, no escaping is performed. Escape `<`, `>`, and `&` characters using `{{ <var>|e }}`, `escape('my text')`, or enable auto-escaping when calling the `render` method.

**Replacing pptx Pictures and Medias**

You can replace dummy pictures or medias in your template with actual ones after rendering.

**Example:**

```python
ppt.replace_pic('dummy_picture.jpg', 'actual_picture.jpg')
ppt.replace_media('dummy_video.mp4', 'actual_video.mp4') 
```

**Get Defined Variables**

Retrieve missing variables after rendering using:

```python
undefined_vars = ppt.get_undeclared_template_variables()
```

**Multiple Rendering**

Create a `PptxTemplate` object once and call `render(context)` multiple times. If using replacement methods, call `reset_replacements()` at the start of the rendering loop.

**Jinja Custom Filters**

Pass a Jinja environment object to `render()` to add custom filters.

**Command-Line Execution**

Generate a pptx from a template and a JSON file using the command line.

**Examples**

Refer to the examples in the `tests/` directory for practical demonstrations.

**Share**

If you appreciate this project, please rate and share it.

**Remember:** This adaptation is based on the provided documentation and general knowledge of python-pptx. Specific implementation details might require further exploration of the **python-pptx-template** library itself.

Please let me know if you have any other questions. 
