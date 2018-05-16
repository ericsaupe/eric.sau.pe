---
templateKey: blog-post
title: Grunt - The JavaScript Task Runner
date: '2013-11-22T15:12:00-08:00'
tags:
  - grunt
  - grunt example
  - grunt less
  - grunt less example
  - grunt setup example
  - grunt uglify
  - grunt uglify example
  - grunt watch
  - grunt watch example
  - grunt.js
  - gruntjs
  - how to set up grunt
  - how to setup grunt
  - how to use grunt
---
Developing for the web can be tedious sometimes.  Not only do you need to develop using multiple languages you need to make things as easy for yourself as you can when it comes to deployment.  One of the most basic things you can do to make your source files ready for production is to minify all of your JavaScript files.  There are a few programs out there that will do it for you but today I want to show you <a title="Grunt" href="http://gruntjs.com/" target="_blank">Grunt</a>.

The great thing about Grunt is it will autocompile and minify your code even as you're developing.  For example, in your templates you set the source for your scripts to the .min.js versions.  Normally once you make a change to the source you'd need to manually minify the file again before refreshing the page.  Grunt will do all of the minifying for you and using one of their plugins can watch your code for changes and automatically recompile when you save.

I'm going to go through a <a href="http://gruntjs.com/getting-started" target="_blank">basic setup for Grunt</a>.  There are many more advanced things you can do with Grunt but I'm just going to show you a basic setupd to recompile your LESS files and minify your JavaScript each time they change.  The first thing you'll want to do is install Grunt.  To install it you'll need NPM and Node.js.  If you already have those installed skip down to step 3.
<ol>
	<li><code>sudo apt-get install npm</code></li>
	<li>Download Node.js from <a href="http://www.google.com/url?q=http%3A%2F%2Fnodejs.org%2F&amp;sa=D&amp;sntz=1&amp;usg=AFrqEzd01UNqDWamEggsGdsc_S6K90kYtQ">http://nodejs.org/</a>
<ol>
	<li><code>tar xvzf node...tar.gz</code></li>
	<li><code>cd node.../</code></li>
	<li><code>sudo ./configure</code></li>
	<li><code>sudo make</code></li>
	<li><code>sudo make install</code></li>
</ol>
</li>
	<li><code>npm install -g grunt-cli</code></li>
	<li>Go to your project folder and run <code>grunt init</code> to create a basic <code>package.json</code> file.  You also need to create an empty <code>Gruntfile.js</code>.  These need to be at the root of your project.</li>
	<li><code>npm install grunt --save-dev</code></li>
	<li><code>npm install grunt-contrib-watch --save-dev</code></li>
	<li><code>npm install grunt-contrib-uglify --save-dev</code></li>
	<li><code>npm install grunt-contrib-less --save-dev</code></li>
</ol>
Step 3 installs the Grunt command-line interface.  Step 4 ensures you have the right files to install your Grunt setup.  Steps 5-8 install the plugins we are going to use in this guide.  Watch is the plugin to watch our files for changes and make something happen when they do.  Uglify is the minification plugin.  Less is what will compile our LESS files to CSS.  The <code>--save-dev</code> automatically puts the plugins into your <code>package.json</code> file.

Now that you have everything installed you need to set up your Gruntfile to do what you want Grunt to do.  Here is a simple example Gruntfile.js.
<pre class="lang:js decode:true ">module.exports = function(grunt) {
  // Project configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    watch: {
        customers: {
            files: ['customers/static/customers/js/source/*'],
            tasks: ['customers'],
        },
        customers_less: {
            files: ['customers/static/customers/less/*'],
            tasks: ['customers_less'],
        },
    },
    less: {
        customers_less: {
            files: [
                {
                    expand: true,
                    cwd: 'customers/static/customers/less',
                    src: '**/*.less',
                    dest: 'customers/static/customers/css',
                    ext: '.css'
                }
            ]
        },
    },
    uglify: {
      options: {
        banner: '/*! &lt;%= pkg.name %&gt; &lt;%= grunt.template.today("yyyy-mm-dd") %&gt; */\n'
      },
      customers: {
        files: [
            {
                expand: true,
                cwd: 'customers/static/customers/js/source',
                src: '**/*.js',
                dest: 'customers/static/customers/js',
                ext: '.min.js',
            }
        ]
      },
    }
  });

  // Plugins
  grunt.loadNpmTasks('grunt-contrib-uglify');
  grunt.loadNpmTasks('grunt-contrib-watch');
  grunt.loadNpmTasks('grunt-contrib-less');

  // Default task(s).
  grunt.registerTask('default', ['uglify', 'less']);
  // Uglify Tasks
  grunt.registerTask('customers', ['uglify:customers']);
  // LESS Tasks
  grunt.registerTask('customers_less', ['less:customers_less']);
};</pre>
Let's walk through that file.  At the top you see <code>grunt.initConfig</code> and in here we will put a dictionary with each key representing either a Grunt keyword or a Grunt plugin.  In the example above you can see there is <code>pkg</code>, <code>watch</code>, <code>less</code>, and <code>uglify</code>.  <code>Pkg</code> reads your package.json for information about your project.  <code>watch</code>, <code>less</code>, and <code>uglify</code> all contain information for the various plugins to know what to compile and where to output it.  I'll quickly go through each plugin and how it is setup.

Watch contains a dictionary with named methods.  I've labeled them after the Django app it relates to in my project but you can name them whatever.  Inside of the labeled section are the keywords <code>files</code> and <code>tasks</code>.  Both are lists and they define what files to watch and what tasks to perform when one of those files have been changed.

Less is structured similarly to Watch but instead of triggering a task it is going to take source files and convert them to CSS files.  It takes one keyword argument with a list of dictionaries with various options.  To use those options you first need to set <code>expand</code> to <code>true</code>.  <code>Cwd</code> is the relative path name to your source files.  <code>Src</code> says where to look for and what type of file they are.  Here we are checking all directories and subdirectories found at <code>cwd</code> for <code>.less</code> files to convert.  <code>Dest</code> is where the CSS files will be outputted.  <code>Ext</code> says what the file extension should be.

Uglify looks almost exactly the same as the less section and that is because it is doing almost the exact same thing, taking source files and outputting files in a destination folder.  I created a sub-folder inside my js folder to house the source files and then send the minified versions to the main js folder.  This allowed me to keep most of my URLs in tact and just add <code>.min.js</code> to the filename.  At the top of uglify there is the options keyword with banner inside of it.  You can customize this to put a small bit of text at the top of your minified files.  Here we have the package name and when it was compiled.

Now that we have all of the plugins configured we need to activate them all.  Place all the names of the plugins you are using under the Plugins section.  After that you create tasks.  Every Gruntfile needs a default task that will be run when you use the command grunt.  I have mine set to do all the things inside of both uglify and less so it compiles all of my code in one command.  Next you'll need to register the task names so watch knows what to run.  Once you've done that you are good to go.  Run the command grunt watch and then change any of your files.  You should see that Grunt automatically redid whatever section it was told to do.

Grunt is a great tool and I have only slightly scratched the surface of what it is capable of.  Please head over to their website to learn more about Grunt and the great things you can do with it.  If you have any tips or tricks with Grunt please comment below.
