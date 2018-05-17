---
templateKey: blog-post
title: ReportLab and Django – Part 3 – Paragraphs and Tables
date: '2014-01-31T10:21:00-08:00'
tags:
  - django reportlab
  - reportlab
  - reportlab black square
  - reportlab black squares
  - reportlab chinese characters
  - reportlab examples
  - reportlab font
  - reportlab foreign characters
  - reportlab table
  - reportlab table example
  - reportlab tables
---
ReportLab is full of different objects that you can place anywhere around the screen.  Following our previous code, we are making a list of elements that we want to draw onto a document.  ReportLab will handle all of the page breaks and lining things up but we can manipulate that a bit to help ourselves get a really clean looking PDF.  You can review the last two guides in the series by clicking these links, <a href="http://ericsaupe.com/reportlab-and-django-part-1-the-set-up-and-a-basic-example/">ReportLab and Django – Part 1 – The Set Up and a Basic Example</a>, <a href="http://ericsaupe.com/reportlab-and-django-part-2-headers-and-footers-with-page-numbers/">ReportLab and Django – Part 2 – Headers and Footers with Page Numbers</a>.

The two most common elements to get your document to look nice are Paragraphs and Tables.  Paragraphs are, just as they sound, a paragraph of text that can contain some formatting to make it look nice.  It can even include images.  You can set a ParagraphStyle for the entire paragraph or put XML tags inside the Paragraph to get various styles throughout one object.
<h3>Paragraphs</h3>
A Paragraph is an element that spans the entire width of the area that it is given.  It holds text or images and will wrap when it reaches the end of the drawable area.  You can set a style for a Paragraph by creating a ParagraphStyle and applying it to your Paragraph.  ReportLab comes with a number of built in styles but I found that I preferred creating my own styles to give me more control.
<pre class="lang:python decode:true">
from reportlab.lib.enums import TA_RIGHT
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont
from django.conf import settings
    # Register Fonts
pdfmetrics.registerFont(TTFont('Arial', settings.STATIC_ROOT + 'fonts/arial.ttf'))
pdfmetrics.registerFont(TTFont('Arial-Bold', settings.STATIC_ROOT + 'fonts/arialbd.ttf'))
    # A large collection of style sheets pre-made for us
styles = getSampleStyleSheet()
    # Our Custom Style
styles.add(ParagraphStyle(name='RightAlign', fontName='Arial', align=TA_RIGHT))
</pre>
In the example you can see that we first register some fonts.  Fonts are a very important feature of ReportLab because it allows you to use any TrueType Fonts you can find.  This is important because if you are going to be using any foreign language characters you'll need a font that supports those characters.  If you don't ReportLab will render them as black squares.

We then got the large amount of style sheets already created for us by ReportLab and then we add to it.  You can see that we add a new ParagraphStyle called 'RightAlign' that will use the Arial font and right align any text inside of it.  Simple enough.  To use it we do just like we did in the example in Part 2 but instead of using a prebuilt stylesheet like Heading1 we just call our own RightAlign.  Below is the example with the changed line highlighted.
<pre class="lang:python mark:15,20 decode:true">def print_users(self):
        buffer = self.buffer
        doc = SimpleDocTemplate(buffer,
                                rightMargin=72,
                                leftMargin=72,
                                topMargin=72,
                                bottomMargin=72,
                                pagesize=self.pagesize)

        # Our container for 'Flowable' objects
        elements = []

        # A large collection of style sheets pre-made for us
        styles = getSampleStyleSheet()
        styles.add(ParagraphStyle(name='RightAlign', fontName='Arial' alignment=TA_RIGHT))

        # Draw things on the PDF. Here's where the PDF generation happens.
        # See the ReportLab documentation for the full list of functionality.
        users = User.objects.all()
        elements.append(Paragraph('My User Names', styles['RightAlign']))
        for i, user in enumerate(users):
            elements.append(Paragraph(user.get_full_name(), styles['Normal']))

        doc.build(elements, onFirstPage=self._header_footer, onLaterPages=self._header_footer,
                  canvasmaker=NumberedCanvas)

        # Get the value of the BytesIO buffer and write it to the response.
        pdf = buffer.getvalue()
        buffer.close()
        return pdf</pre>
There are a lot you can do within a Paragraph as well using inline tags.  If you wanted to make changes to a certain section of text and wanted to change it to be a different color, font, or size you can by using the &lt;font&gt; tags.  Adding an image is equally as simple by adding the &lt;img&gt; tag and specifying a file location.  Below is an example of a Paragraph with the same style we created above but with altered font and image added inline.
<pre class="lang:python decode:true">elements.append(Paragraph('&lt;font name="Arial-Bold" color="red"&gt;My&lt;/font&gt; User Names&lt;img src="smiley.jpg" height="5"&gt;', styles['RightAlign']))</pre>
This creates a right aligned paragraph that has a bold red My followed by a normal Arial User Names and then a image of a smiley face with a height of 5.
<h3>Tables</h3>
Tables are the biggest help ever for getting things all lined up.  A Table is exactly like every other instance of a table you know.  It has rows and columns and you can specify their size and what is inside of them.  Tables by default will grow to contain whatever object is inside of them.  You can explicitly set the widths and heights of the columns and rows but your text/images can still overflow onto the outside of them.  Getting the widths and heights right is key.  Let's make a basic table with some user data and set some table styles to make it look great.
<pre class="lang:default mark:22-31 decode:true">
def print_users(self):
        buffer = self.buffer
        doc = SimpleDocTemplate(buffer,
                                rightMargin=72,
                                leftMargin=72,
                                topMargin=72,
                                bottomMargin=72,
                                pagesize=self.pagesize)

        # Our container for 'Flowable' objects
        elements = []

        # A large collection of style sheets pre-made for us
        styles = getSampleStyleSheet()
        styles.add(ParagraphStyle(name='RightAlign', fontName='Arial' alignment=TA_RIGHT))

        # Draw things on the PDF. Here's where the PDF generation happens.
        # See the ReportLab documentation for the full list of functionality.
        users = User.objects.all()
        elements.append(Paragraph('My User Names', styles['RightAlign']))

        # Need a place to store our table rows
        table_data = []
        for i, user in enumerate(users):
            # Add a row to the table
            table_data.append([user.get_full_name(), user.username, user.last_login]))
        # Create the table
        user_table = Table(table_data, colWidths=[doc.width/3.0]*3)
        user_table.setStyle(TableStyle([('INNERGRID', (0, 0), (-1, -1), 0.25, colors.black),
                                        ('BOX', (0, 0), (-1, -1), 0.25, colors.black)]))
        elements.append(user_table)
        doc.build(elements, onFirstPage=self._header_footer, onLaterPages=self._header_footer,
                  canvasmaker=NumberedCanvas)

        # Get the value of the BytesIO buffer and write it to the response.
        pdf = buffer.getvalue()
        buffer.close()
        return pdf
</pre>
This will create a table with all of our users on it, their usernames, and when they last logged in.  The table will be lined, that's what the INNERGRID and BOX style does.  You can see that the table will span the entire page and that each column will take up a third of the document's width.  You can customize these for varying column widths.  There are a lot of various customization options for tables.  Read them on the ReportLab docs.

I hope this was helpful.  I spent a lot of time figuring these things out as the documentation and examples are sparse.  If there are any problems you have that you want to know about let me know in the comments below.
