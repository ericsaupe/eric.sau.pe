---
templateKey: blog-post
title: ReportLab and Django - Part 1 - The Set Up and a Basic Example
date: '2014-01-14T15:18:00-08:00'
tags:
  - django
  - django generate pdf
  - django pdf
  - django reportlab
  - django reportlab examples
  - django reportlab pdf
  - reportlab
  - reportlab django
  - reportlab examples
  - reportlab getting started
---
It's been far too long since my last post.  A lot has happened and the holidays make things so busy but I'm back and ready to get back in to the swing of things.  To start off the new year I'm going to do a series of posts relating to <a href="http://www.reportlab.com/" target="_blank">ReportLab</a> specifically with its application to <a href="https://docs.djangoproject.com/en/dev/howto/outputting-pdf/" target="_blank">Django</a>.  I want to go over a few of the pitfalls I've experienced and how to get things set up.  ReportLab is notoriously tricky to get working and the documentation seems sparse to say the least.  I hope these posts help not only myself remeber how I did all of this but others who are struggling with the same problems.
<h2>ReportLab</h2>
The problem is simple, <a href="https://docs.djangoproject.com/en/dev/howto/outputting-pdf/" target="_blank">how to generate PDFs using Django</a>.  In our on going rewrite of our system at work we are getting to PDF generation.  The old system uses <a href="https://code.google.com/p/wkhtmltopdf/" target="_blank">wkhtmltopdf</a> which is great if you just want something to render HTML out to a PDF and you don't give much care to speed or formatting.  Wkhtmltopdf suffers from having to use HTML to format everything on the page.  This is limiting because it is difficult to add headers and footers, positioning objects on the page is tricky, and we have a bad problem with it printing blank pages due to random overflow of the elements.  We needed a better solution and we have found it with ReportLab.  ReportLab has some HTML support, and we'll get to that in a later segment, but for the most part you are drawing elements, text, images, etc. directly onto a canvas that is then rendered as a PDF.  The speed increase for us was unbelievable.  The formatting and control that we get is unmatched.  Most importantly, the font support has given us cleaner paperwork than we ever thought possible.  I guess what I'm trying to say is ReportLab is great...once you get it to work.  <a href="http://www.reportlab.com/docs/reportlab-userguide.pdf" target="_blank">ReportLab Documentation</a> is broad but not specific enough for a lot of our concerns.  You'll find yourself heading to Google for even the smallest of questions.  I hope to alleviate many of the common pitfalls with this series.
<h2>Getting Set Up</h2>
Start by installing reportlab into your <a title="Developing with Virtualenv" href="http://ericsaupe.com/developing-with-virtualenv/">virtual environment</a> with <code>pip install reportlab</code>. The first thing to do is get a robust class set up for doing all of your printing.  I created a printing.py file that houses all of my basic ReportLab set up and functions.  Branching off from there I have my individual pages and paperwork that get combined and output to the user.  Let's start with a simple printing.py file.
<pre><code>from reportlab.lib.pagesizes import letter, A4

class MyPrint:
    def __init__(self, buffer, pagesize):
        self.buffer = buffer
        if pagesize == 'A4':
            self.pagesize = A4
        elif pagesize == 'Letter':
            self.pagesize = letter
        self.width, self.height = self.pagesize</code></pre>
As you can see my class is very simple. I pass in a buffer to house my data that will be returned to the user, I'll show this once we get set up, and a page size. ReportLab has a lot of built in page sizes but you can even set your own if you'd like by using your own width and height variables.
<h2>Simple Example - Printing Users</h2>
Now let's add a simple method to print out all of the users of our application into a PDF and return it to the browser.  Inside of the MyPrint class I'm adding a method that will print the list of users.
<pre>from reportlab.platypus import SimpleDocTemplate, Paragraph
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib.enums import TA_CENTER
from django.contrib.auth.models import User

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
        styles.add(ParagraphStyle(name='centered', alignment=TA_CENTER))

        # Draw things on the PDF. Here's where the PDF generation happens.
        # See the ReportLab documentation for the full list of functionality.
        users = User.objects.all()
        elements.append(Paragraph('My User Names', styles['Heading1']))
        for i, user in enumerate(users):
            elements.append(Paragraph(user.get_full_name(), styles['Normal']))

        doc.build(elements)

        # Get the value of the BytesIO buffer and write it to the response.
        pdf = buffer.getvalue()
        buffer.close()
        return pdf
</pre>
The code is commented nicely but let's walk through it.  We start by taking our buffer and putting it into what is a SimpleDocTemplate.  This is a ReportLab object that will generate a PDF with some standard values.  There are a ton of customizations you can make and even create your own document templates but for now let's stick with the simple.  We set the margins for the page and the page size.  You can use inches or millimeters too if you'd like, ReportLab has a few built in measurement sizes that just need to be imported.

Next we have a list of elements.  This list will hold all of the ReportLab Flowables that will be generated on the PDF itself.  A Flowable is an object that has space and size on a PDF.  This can be a block of text, a table, an image, or you can even create a custom Flowable.  Whatever we put in this list will be generated in order on our PDF.

After that you'll see some styles that we use.  These styles are going to be used for various elements like Paragraph styling.  After we get the style sheets I've added a new one that simply centers text and nothing more.

Now we come to the part where we are going to add elements to our document to be rendered.  I've used Django to grab my list of users and I loop through them one at a time to add their name to a Paragraph flowable.  A Paragraph flowable will take up the entire width of the area you give it.  In our case we have not confied the Paragraph to the inside of a table or to a column so each Paragraph will fill in the entire width of the document.

Once we have added all of our elements we simply build the doc.  This renders the PDF and places all PDF data into that buffer we gave the doc when we initiated it.  Close the buffer for good practice and return the contents of the PDF to the user.  Simple enough.  To run all of the code that I have shown I have a view in my views.py file that will run all of this and you can see how to access the printing.py class and method and output your PDF to your users.
<pre>from myapp.printing import MyPrint
from io import BytesIO

@staff_member_required
def print_users(request):
    # Create the HttpResponse object with the appropriate PDF headers.
    response = HttpResponse(content_type='application/pdf')
    response['Content-Disposition'] = 'attachment; filename="My Users.pdf"'

    buffer = BytesIO()

    report = MyPrint(buffer, 'Letter')
    pdf = report.print_users()

    response.write(pdf)
    return response
</pre>
That's all there is to it. I hope this was enlightening on at least how to get started. Next time I will explain how to add headers and footers to your documents as well as a surprisingly tricky to find way of adding page numbers.

If you have anything you'd like to learn about for ReportLab and Django please leave a comment below.
<h3><a href="http://ericsaupe.com/reportlab-and-django-part-2-headers-and-footers-with-page-numbers/">ReportLab and Django – Part 2 – Headers and Footers with Page Numbers</a></h3>
