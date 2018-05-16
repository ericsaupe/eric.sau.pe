---
templateKey: blog-post
title: jQuery Drag and Drop Image Upload
date: '2014-01-17T15:20:00-08:00'
tags:
  - django
  - django drag and drop files
  - django drag and drop images
  - django jquery drag and drop files
  - django upload multiple images
  - jquery
  - jquery drag and drop files
  - jquery drag and drop images
  - jquery file upload
  - jquery image upload
  - jquery multiple image upload
---
As we continue to push forward on development our users have asked for a robust way of dragging and dropping images and linking them to a work order.  I was putting this off for a while because I thought it would be a pretty difficult task but after a few Google searches I found a great jQuery plugin from blueimp called <a title="jQuery File Upload Source" href="https://github.com/blueimp/jQuery-File-Upload" target="_blank">jQuery-File-Upload</a>.  Take a look at their <a title="jQuery File Upload Demo" href="http://blueimp.github.io/jQuery-File-Upload/" target="_blank">demo</a> and check out how awesome it is.  We didn't need any of the fancy bells and whistles that came with the Basic Plus UI so this tutorial focuses on just the Basic but you can scale yours how you'd like.  The tutorial will also focus on getting it to work and save to a MySQL database through Django.

To start off let's get the drag and drop area set up on our page.  To do that you'll need to add these things to your HTML template, Django tags are added because that is what we are using to render the page.

<strong>image_upload.html</strong>
<pre class="lang:default decode:true">{% block css %}
    &lt;!-- jQuery Upload Files --&gt;
    &lt;link rel="stylesheet" href="{% static 'jQuery-File-Upload/css/style.css' %}"&gt;
    &lt;link rel="stylesheet" href="{% static 'jQuery-File-Upload/css/jquery.fileupload.css' %}"&gt;
{% endblock %}

{% block js %}
    &lt;!-- jQuery Upload Files --&gt;
    &lt;script src="{% static 'jQuery-File-Upload/js/vendor/jquery.ui.widget.js' %}"&gt;&lt;/script&gt;
    &lt;script src="{% static 'jQuery-File-Upload/js/jquery.iframe-transport.js' %}"&gt;&lt;/script&gt;
    &lt;script src="{% static 'jQuery-File-Upload/js/jquery.fileupload.js' %}"&gt;&lt;/script&gt;
    &lt;script&gt;
        /*jslint unparam: true */
        /*global window, $ */
        $(function () {
            'use strict';
            // Change this to the location of your server-side upload handler:
            var url = '/image/upload/';
            $('#fileupload').fileupload({
                url: url,
                dataType: 'json',
                done: function (e, data) {
                    $.each(data.result.files, function (index, file) {
                        $('&lt;p/&gt;').text(file.name).appendTo('#files');
                        $('&lt;p/&gt;').html("&lt;img src='" + file.url + "' width=160px/&gt;").appendTo('#files');
                    });
                },
                progressall: function (e, data) {
                    var progress = parseInt(data.loaded / data.total * 100, 10);
                    $('#progress .progress-bar').css(
                        'width',
                        progress + '%'
                    );
                }
            }).prop('disabled', !$.support.fileInput)
                .parent().addClass($.support.fileInput ? undefined : 'disabled');
        });
    });
    &lt;/script&gt;
{% endblock %}</pre>
With the code above you should be ready to drag and drop files immediately, at least from the front end perspective. Now let's set up the server side of things. Make sure you add your URL from the fileupload to your urls.py file. Then create a view for that URL that looks something like this.

<strong>views.py</strong>
<pre class="lang:python decode:true">@staff_member_required
def image_upload(request):
    response = {'files': []}
    # Loop through our files in the files list uploaded
    for image in request.FILES.getlist('files[]'):
        # Create a new entry in our database
        new_image = Pictures(name=image.name)
        # Save the image using the model's ImageField settings
        filename, ext = os.path.splitext(image.name)
        new_image.picture.save("%s-%s%s" % (image.name, datetime.datetime.now(), ext), image)
        new_image.save()
        # Save output for return as JSON
        response['files'].append({
            'name': '%s' % image.name,
            'size': '%d' % image.size,
            'url': '%s' % new_image.picture.url,
            'thumbnailUrl': '%s' % new_image.picture.url,
            'deleteUrl': '\/image\/delete\/%s' % image.name,
            "deleteType": 'DELETE'
        })

    return HttpResponse(json.dumps(response), content_type='application/json')</pre>
The view handles all of the uploaded files one at a time and saves them to wherever we would like. In this case we are making a database entry that houses the file in an ImageField and we have a CharField for a name that we can change without altering the file path used in the ImageField. The JSON output is important as it will read the data sent back and can output our newly added images right to the screen.  That's all there is to it.

Now there may be some circumstances where you would like to add more data to the request than just the images you have dragged over.  In our case we wanted the work order number to be attached as well and saved in the file name.  Using jQuery File Upload we can add data anytime something is dropped by adding a bind call to all fileuploadsubmit method calls.  I added the following lines of code in the HTML template just underneath the <code>var url</code> line.
<pre class="lang:js decode:true">$('#fileupload').bind('fileuploadsubmit', function (e, data) {
    data.formData = {'id': $("#workorder").data("id")};
    var active = $("#workorder).data("active");
    if (!active) {
        return false;
    }
});</pre>
This will add the workorder id to the POST request data on every call using the fileupload. Another thing to note is you can add a check to see if you really want to submit the uploads like I have done. If the workorder is not active do not upload any images. You can customize this any way you'd like since it is all just JavaScript and jQuery code.

Another problem I ran into was having multiple file upload elements on the same page. This was really frustrating and reading through their documentation they had a fix that worked pretty well with a couple of tweaks.

First, to make all of the drag and drop areas work independently of one another you change the fileupload elements from having an id of fileupload to a class of fileupload. This will need to change on the above code for bind as well. But you can't simply change it on the $('#fileupload').fileupload() becuase then it will upload once for every drag and drop element on the page. You instead have to take it a step back and add these lines.
<pre class="lang:js decode:true">$('.fileupload').each(function () {
    $(this).fileupload({
        dropZone: $(this),
        url..., });
});</pre>
This allows every fileupload classed element to take files individually. But this was annoying because users had to drop the files right on the button to make it work. As you can see there is a new parameter we've added called dropZone. This tells the fileupload what area of the page to watch for. If you want to make this area bigger or smaller that is totally up to you. I changed mine to <code>dropZone: $(this).parents(".panel-body")</code> to allow the user to drop images anywhere inside the panel body and they will start the upload.

I hope this was useful to someone. We are actually going in a different direction with our image uploads but we may need this in the future and I wanted to document how I got it all working with both jQuery File Upload and Django.

EDIT

Thanks to <a href="https://plus.google.com/u/1/112933314802331259935/posts" target="_blank">Tuan Nguyen</a> for pointing out that in its default configuration each image is uploaded in its own request.  To join these into one request, open up the <code>jQuery-File-Upload/js/jquery.fileupload.js</code> file and search for the line <code>singleUploadFiles:</code> and set it to <code>true</code>.  You can also limit the number of files to be uploaded right below by changing <code>limitMultiFileUploads:</code>.
