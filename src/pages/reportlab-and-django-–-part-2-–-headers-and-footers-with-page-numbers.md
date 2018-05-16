---
templateKey: blog-post
title: ReportLab and Django – Part 2 – Headers and Footers with Page Numbers
date: '2014-01-21T15:30:00-08:00'
tags:
  - django
  - django pdf
  - django pdf footer
  - django pdf header
  - django reportlab
  - django reportlab examples
  - django reportlab footer
  - django reportlab header
  - django reportlab header footer
  - django reportlab header footer example
  - django reportlab page number
  - django reportlab page numbers
  - django reportlab pdf
  - reportlab
  - reportlab footer
  - reportlab footer page number
  - reportlab header
  - reportlab header footer
  - reportlab header footer example
  - reportlab page number
---
Last time we looked at how to generate a very simple PDF using ReportLab and Django, <a title="Edit “ReportLab and Django – Part 1 – The Set Up and a Basic Example”" href="http://ericsaupe.com/reportlab-and-django-part-1-the-set-up-and-a-basic-example/">ReportLab and Django – Part 1 – The Set Up and a Basic Example</a>.  This time let's make the PDF a little bit more interesting with some headers and footers.  ReportLab gives a pretty good amount of control when it comes to adding headers and footers to your PDF.  To start off let's bring back the simple __init__ method we had from last time and add a header/footer method to it.
<pre class="lang:python decode:true">from reportlab.lib.pagesizes import letter, A4
from reportlab.lib.styles import getSampleStyleSheet

class MyPrint:
    def __init__(self, buffer, pagesize):
        self.buffer = buffer
        if pagesize == 'A4':
            self.pagesize = A4
        elif pagesize == 'Letter':
            self.pagesize = letter
        self.width, self.height = self.pagesize

    @staticmethod
    def _header_footer(canvas, doc):
        # Save the state of our canvas so we can draw on it
        canvas.saveState()
        styles = getSampleStyleSheet()

        # Header
        header = Paragraph('This is a multi-line header.  It goes on every page.   ' * 5, styles['Normal'])
        w, h = header.wrap(doc.width, doc.topMargin)
        header.drawOn(canvas, doc.leftMargin, doc.height + doc.topMargin - h)

        # Footer
        footer = Paragraph('This is a multi-line footer.  It goes on every page.   ' * 5, styles['Normal'])
        w, h = footer.wrap(doc.width, doc.bottomMargin)
        footer.drawOn(canvas, doc.leftMargin, h)

        # Release the canvas
        canvas.restoreState()</pre>
Now we have a method that will draw things onto every page of our header and footer.  Next we need to actually call that method.  Remember last time where we built the document using doc.build(elements) and ReportLab built a PDF using the flowable elements in the list?  Well this time we are just going to add some more parameters to let the build process know that it needs to also draw out the header and footer on every page.  Below is our print_users method from last time with the new parameters added to the build command shown in bold.
<pre class="lang:python mark:24 decode:true">from reportlab.lib.units import inch

def print_users(self):
        buffer = self.buffer
        doc = SimpleDocTemplate(buffer,
                                rightMargin=inch/4,
                                leftMargin=inch/4,
                                topMargin=inch/2,
                                bottomMargin=inch/4,
                                pagesize=self.pagesize)

        # Our container for 'Flowable' objects
        elements = []

        # A large collection of style sheets pre-made for us
        styles = getSampleStyleSheet()
        styles.add(ParagraphStyle(name='centered', alignment=TA_CENTER))

        # Draw things on the PDF. Here's where the PDF generation happens.
        # See the ReportLab documentation for the full list of functionality.
        users = User.objects.all()
        elements.append(Paragraph('My User Names', styles['Heading1']))
        for i, user in enumerate(users):
            elements.append(Paragraph(user.get_full_name(), styles['Normal']))

        doc.build(elements, onFirstPage=self._header_footer, onLaterPages=self._header_footer)

        # Get the value of the BytesIO buffer and write it to the response.
        pdf = buffer.getvalue()
        buffer.close()
        return pdf</pre>
That's it. Now our print_users method will print out a few lines on both the header and the footer of every page. You'll notice there are two parameters, onFirstPage and onLaterPages. This allows you to have different headers and footers on the front page, which could be a title page, and on later pages that would require page numbers. Speaking of page numbers let's get into that.

Page numbers on every page was a big challenge for me to find anything on. It was easy enough to put Page X on every page but what I really wanted was Page X of Y and that "of Y" portion was tricky. Luckily, I've figured it out and I will present it here for you now.

You can alter the way that the PDF is built using the canvasmaker parameter in the doc.build() process. These canvasmakers can help you inject new things on every page without using a header or footer. Below is the code that I found somewhere online to print the Page X of Y on every page down near the footer. I have placed this all within the printing.py file I created last time.
<pre><code>from reportlab.pdfgen import canvas
from reportlab.lib.units import mm

class NumberedCanvas(canvas.Canvas):
    def __init__(self, *args, **kwargs):
        canvas.Canvas.__init__(self, *args, **kwargs)
        self._saved_page_states = []

    def showPage(self):
        self._saved_page_states.append(dict(self.__dict__))
        self._startPage()

    def save(self):
        """add page info to each page (page x of y)"""
        num_pages = len(self._saved_page_states)
        for state in self._saved_page_states:
            self.__dict__.update(state)
            self.draw_page_number(num_pages)
            canvas.Canvas.showPage(self)
        canvas.Canvas.save(self)

    def draw_page_number(self, page_count):
        # Change the position of this to wherever you want the page number to be
        self.drawRightString(211 * mm, 15 * mm + (0.2 * inch),
                             "Page %d of %d" % (self._pageNumber, page_count))</code></pre>
To use this we just add a new parameter to our doc.build() command. You can see it added below in bold.
<pre class="lang:default mark:24-25 decode:true">def print_users(self):
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
        styles.add(ParagraphStyle(name='centered', alignment=TA_CENTER))

        # Draw things on the PDF. Here's where the PDF generation happens.
        # See the ReportLab documentation for the full list of functionality.
        users = User.objects.all()
        elements.append(Paragraph('My User Names', styles['Heading1']))
        for i, user in enumerate(users):
            elements.append(Paragraph(user.get_full_name(), styles['Normal']))

        doc.build(elements, onFirstPage=self._header_footer, onLaterPages=self._header_footer,
                  canvasmaker=NumberedCanvas)

        # Get the value of the BytesIO buffer and write it to the response.
        pdf = buffer.getvalue()
        buffer.close()
        return pdf</pre>
There you have it. Now you have a great header and footer on every page, or every page after the first, and page numbers on all of them. How fantastic is that? Next time we will go a bit deeper into Tables and how to use them to line everything up really nicely. They will be usable wherever you want a flowable including your new headers and footers.
