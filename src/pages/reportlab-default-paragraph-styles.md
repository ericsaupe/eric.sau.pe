---
templateKey: blog-post
title: ReportLab Default Paragraph Styles
date: '2014-03-26T10:30:00-07:00'
tags:
  - reportlab
  - reportlab getSampleStyleSheet
  - reportlab paragraph styles
  - reportlab styles
---
I have never found a good list of the default styles that ReportLab gives you when you run getSampleStyleSheet() so I wrote a method to spit them out and what they look like.  Below is that list along with the PDF output of each of the styles.  They are grouped into two categories, byAlias and byName.  Normally I just use the name but you can use either.
<ul>
	<li>byAlias
<ul>
	<li>'title': &lt;ParagraphStyle 'Title'&gt;</li>
	<li>'h1': &lt;ParagraphStyle 'Heading1'&gt;</li>
	<li>'h2': &lt;ParagraphStyle 'Heading2'&gt;</li>
	<li>'h3': &lt;ParagraphStyle 'Heading3'&gt;</li>
	<li>'h4': &lt;ParagraphStyle 'Heading4'&gt;</li>
	<li>'h5': &lt;ParagraphStyle 'Heading5'&gt;</li>
	<li>'h6': &lt;ParagraphStyle 'Heading6'&gt;</li>
	<li>'bu': &lt;ParagraphStyle 'Bullet'&gt;</li>
	<li>'df': &lt;ParagraphStyle 'Definition'&gt;</li>
</ul>
</li>
	<li>byName
<ul>
	<li>'Title': &lt;ParagraphStyle 'Title'&gt;</li>
	<li>'Heading1': &lt;ParagraphStyle 'Heading1'&gt;</li>
	<li>'Heading2': &lt;ParagraphStyle 'Heading2'&gt;</li>
	<li>'Heading3': &lt;ParagraphStyle 'Heading3'&gt;</li>
	<li>'Heading4': &lt;ParagraphStyle 'Heading4'&gt;</li>
	<li>'Heading5': &lt;ParagraphStyle 'Heading5'&gt;</li>
	<li>'Heading6': &lt;ParagraphStyle 'Heading6'&gt;</li>
	<li>'Bullet': &lt;ParagraphStyle 'Bullet'&gt;</li>
	<li>'Definition': &lt;ParagraphStyle 'Definition'&gt;</li>
	<li><span style="line-height: 1.5em;">'Normal': &lt;ParagraphStyle 'Normal'&gt;</span></li>
	<li>'Italic': &lt;ParagraphStyle 'Italic'&gt;</li>
	<li><span style="line-height: 1.5em;">'BodyText': &lt;ParagraphStyle 'BodyText'&gt;</span></li>
	<li><span style="line-height: 1.5em;">'Code': &lt;ParagraphStyle 'Code'&gt;</span></li>
</ul>
</li>
</ul>
