Title: Install pelican and write~
Date: 2015-07-07 09:34
Category: Development
Tags: python, pelican

# Quick Start
The default recommendation of GitHub Pages is jekyll, but I prefer Python than Ruby to write something. As a result, I found pelican to write my blog on GitHub Pages. Here, <http://docs.getpelican.com/en/3.6.0/quickstart.html>

The installation is quite easy in pythonic way:

    c:\> pip install pelican markdown

The pip will automatically install other dependencies for you such as jinja2, etc. 

Then I can start my project, first, create an appropriately-named directory and switch to it (we call it site directory):

    mkdir focusheart.github.io
    cd focusheart.github.io

Create basic files via the `pelican-quickstart` command:

    pelican-quickstart

Most of the questions have default values in brackets. When finished, we can start writing the first article:

    cd content
    vim hello-world.md

The basic content may like this:

	Title: Hello World
	Date: 2015-07-07 10:20
	Category: Development
	Tags: pelican, python
	Slug: hello-world
	
	Hello, world! 

The syntax follows Markdown Extensions. When done writing, generate the HTML by the following command in site  directory:

    pelican content

Then we can launch a web server to see the article.

    cd output
    python -m pelican.server

Preview our site by navigating to <http://localhost:8000> in my browser.

# More Configuration

In order to work with GitHub Pages, I changed some config to make it easier to sync all sources and generated contents with GitHub. The pelican config file is hosted at <https://github.com/focusheart/focusheart.github.io/blob/master/pelicanconf.py>.

More then default configs, I modified and added some following:

    SITEURL = '' # I found this works well for me
    OUTPUT_PATH = '' # output everything in root dir
    STATIC_EXCLUDES = ['images'] # seems no use
	ARTICLE_EXCLUDES = ['images'] # the same as above
	PAGE_EXCLUDES = ['images'] # the same
	STATIC_EXCLUDE_SOURCES = False # the same
    
    # I changed the default article url and save file 
    ARTICLE_URL = 'posts/{date:%Y}/{slug}.html'
	ARTICLE_SAVE_AS = 'posts/{date:%Y}/{slug}.html'
	PAGE_URL = 'pages/{slug}.html'
	PAGE_SAVE_AS = 'pages/{slug}.html'

The above config works well, but the images in `content/images` will be duplicated and sync to GitHub. Then I add `.gitignore` to ignore the generated directory `images` and other files:

    /images/
    *.pyc

The detail in <https://github.com/focusheart/focusheart.github.io/blob/master/.gitignore>

The most of my work done, and I will try my best to update this blog~