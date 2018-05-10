---
templateKey: blog-post
title: Django Dynamic ModelForm Using Closure
date: '2013-05-23T11:38:00-07:00'
tags:
  - closure
  - database
  - django
  - dynamic
  - meta
  - mysql
  - software
  - web development
  - wikipedia
---
At my job we are currently undergoing a total rewrite of their old web-based data entry system.  It's written in Django and works pretty-alright we are just going to make it better.  When it was first written the company was small and very few tests were being done.  This caused the initial design to lack foresight into the growth of the project.  Another problem with this current system is that it was an almost direct port of their old Microsoft Access version they had been running before.  The company is international and a web based system is much better and easier for the employees to use but wasn't designed with growth in mind.

With a better understanding of how the systemis being used and what the users wanted it to do we have been in the beginning phases of development for about three weeks now.  Parts of the site are ready to go live within a week but the major parts are left.

One obstacle that was brought up by one of the software engineers today was the idea of creating a custom generic form that could be reused in various ways throughout the site.  The trick was going to be with saving to a dynamic model.  Django has few great built-in ways of creating a form based on a model on our MySQL database.  The problem is there isn't an easy way to pass it a model to base the form off of.  Theoretically, you should create a model form for each of your models regardless.  We felt like we could get more out of writing our own custom generic form though.

To accomplish this hurdle I came across closures.  I'd never heard of closures before and if you are in the mood the <a title="Closure (Computer Science)" href="http://en.wikipedia.org/wiki/Closure_(computer_science)" target="_blank" rel="noopener">Wikipedia article</a> does a fine job explaining it.  Essentially we were going to use a closure around our generic model form class to pass it any model we pleased.

Following <a title="Django forms: Defining model in Meta on the fly" href="http://stackoverflow.com/q/10782340/575985" target="_blank" rel="noopener">this example from StackOverflow</a> (<a title="Dynamically update ModelForm's Meta class" href="http://stackoverflow.com/q/297383/575985" target="_blank" rel="noopener">another example</a>) we came up with something like the following:
<pre><code>def generic_model_form(custom_model):
    class GenericModelForm(forms.ModelForm):
         ...
        class Meta:
            model = custom_model

    return GenericModelForm

form = generic_model_form(this_model)()
</code></pre>
This worked out great.  We were able to instantly see the forms generated to our templates using this new way.  Unfortunately, we spent so long on this and just thinking about how to generate and save all of these forms that we went completely back to the drawing board with all of the models.  I'd rather have simple, easy-to-understand code than overly-generic, complex code.  I just found the whole idea of closures, which I believe are similar to lambda expressions which I have used before, very fascinating and we were able to use them, if even just for a moment, today in our daily tasks and creations.

-Eric
