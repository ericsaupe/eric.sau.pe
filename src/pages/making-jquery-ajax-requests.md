---
templateKey: blog-post
title: Making jQuery Ajax Requests
date: '2013-10-22T15:02:00-07:00'
tags:
  - ajax
  - ajax requests
  - jquery ajax
  - jquery ajax requests
---
In a lot of my other posts I show sample code that makes heavy use of ajax calls and today I wanted to take a step back and explain what exactly are ajax requests, when you should use them, and how to get the job done.  First, <a title="Ajax" href="http://en.wikipedia.org/wiki/Ajax_(programming)" target="_blank">ajax</a> stands for Asynchronous JavaScript and XML which may still be a bit too technical but essentially what it does is allow you to make a request to the server for data without having to refresh the page.  Obviously this is a very useful tool to have no matter what you are doing on the web.  No one likes to have to wait for the entire page to reload all of the images and content just because you clicked a button to show more data on a section of the page.

Ajax calls should be used anywhere you want to get data without doing a hard refresh.  This can range from something small like updating a table every ten seconds automatically or to something complicated like automatic submission and verification of individual form fields.  One way to leverage the asynchronous nature of these calls is to load your main html page that merely holds placeholder divs with no content in them.  Once that very light page is loaded you make a series of ajax requests to the server to fill in those various portions of the page.  This gives the user a very quick response time so they see that your site is up and running and other, more intensive, portions of the site can be loaded when they get processed.  Ever gone to a site and after the page has loaded there is still a spinning wheel on some section of the page?  That's an ajax call running and they have let you know something will be there soon.

To <a title="jQuery Ajax" href="http://api.jquery.com/jQuery.ajax/" target="_blank">make an ajax call using jQuery</a> requires very little code but you need to understand the back and forth interaction to make sure you use and display the data correctly.  An ajax call consists of the method call and a dictionary of parameters to be used.  There are many optional ones but I will go over the more commonly used ones.
<pre><code>$("#blogForm").submit(function(e){
    $.ajax({
        url: 'savepost/',
        type: 'POST',
        data: $(this).serialize(),
        error: function(data) {
            alert("An error has occurred. " + data);
        },
        success: function(data) {
            $("#postsTable tr:last").after(data);
        },
    });
});</code></pre>
In the example above we are submitting a form and then adding the returned html data to the table.  This allows us to dynamically add to an HTML table without needing to refresh the page every time.  Let's go through each of the parameters in the dictionary that is passed with the ajax request.
<ul>
	<li>URL - This is obviously the URL where the request is going to be made.  It can be relative, as it is in this example, or an explicit URL like http://www.google.com/.</li>
	<li>TYPE - How the request is being sent.  Generally speaking a GET request is used when you are reading from the database and requesting information and a POST request is one in which you are doing a write or update to the database.  In our case here we are adding a post so that would fall under the write category and we do a POST.</li>
	<li>DATA - The parameters that are going to be held in the request.  This includes all of your fields in your form, which is what I'm doing in the example above, or any GET parameters if you use type GET.  The .serialize() command will take all of your named fields in a form and put them in a dictionary to pass in an ajax request.</li>
	<li>ERROR - If the server responds with an error code of any kind this is what will be triggered.  The data variable that is passed to the error function contains all of the data sent back from the server.</li>
	<li>SUCCESS - When the server responds with a 200 code, a success, this will be triggered.</li>
</ul>
You'll notice that the error and success sections are functions that use a data variable.  That data variable is what is sent back from the server.  It can be anything you'd like it to be.  In the example above the server is sending back pure HTML.  If you want to use other things like lists and dictionaries you'll need to specify another parameter called dataType and set it to <a href="http://en.wikipedia.org/wiki/JSON" target="_blank">JSON</a>.  Then have your server send back a JSON string and you'll be all set.  Because they are functions you can really do whatever you'd like at the point of return.

Ajax calls are crazy powerful and unbelievably useful for nearly aspect of web development.  Play around with it and make your websites better.  There are a lot of <a title="jQuery Ajax" href="http://api.jquery.com/jQuery.ajax/" target="_blank">ajax parameters</a> you can use with your jQuery call.  I only touched on a few of the more important ones here.
