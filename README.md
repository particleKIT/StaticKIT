# Build your KIT .public_html webpage

This is a minimalistic and lightweight workframe for adopting the official KIT website design with static HTML pages and jQuery.
All the configurations come within one simple [config.yml](example/config.yml). Content of pages is separated in single [files](example/pages/).
The resulting pages are build from jinja2 templates with a simple python run.

## Requirements
 * python3 (python3-jinja2, python3-yaml),
 * a webserver with .htaccess support (optional).

## Usecases
Many institutes at the KIT provide a ``.public_html`` directory in the homedir of their members. There you can put static htmls only. Mostly, this leads to a chaos of many html files... 
The intention of this project is, to have a minimalistic framework for building simple website structures without batteling with thousands of html files and just concentrating on the content.

## Quickstart
 * get a clone of this repository: ``git clone https://github.com/particleKIT/StaticKIT.git``, optionally symlink the [./publish.py](publish.py) into a place withn your ``$PATH`` (e.g. ~/bin).
 * initialize a new template project with ``publish --init ~/.private_html``
 * for every page/template that makes use of variables/jinja2 syntax create a ``.html`` file at ``~/.private_html/pages/`` and fill it with your (html or markdown) content,
 * every page that should be shown in the navigation needs an entry in the ``~/.private_html/config.yml``, the syntax is as follows: 
  
  ```yaml
   navigation:
        - file: filename (without html extension)
          title: filetitle (the name shown in the navigation)
          order: 1  (the position of the page in the navigation)
  ```
  
 * run ``publish --input ~/.private_html --output ~/.public_html``
 * thats it! The complete page is navigated from the newly created ``index.html``, all the jinja templates are proccessed with the objects from the config.yml.
 * also have a look at the output of ``publish --help``

## Infobox
You can add additional infoboxes on the right side of the page. Fill the ``infobox`` dict with appropriate informations, every new subdict (starting with a ``-``) marks a new infobox:
```yaml
infobox:
    - title: Info-Box
      text: "A simple list
          <ul>
          <li>item 1</li>
          <li>another item</li>
          <li>third item</li>
          </ul>
          "
    - title: Info-Box2
      text: "another infobox"
```
If you need more space and dont want to show the right column at all, simply set the ``showboxes: false`` variable the config.yml. 

## Linking your pages
Loading page content from specific files under ``/pages/*.html`` is triggered by appending a ``#filename`` (without .html extension) in the addressbar. Thus you can give others a link to one of your pages by copy&paste the resulting url from the navigation/browser address bar.


## Recycling variables
People using ansible (which mixes yaml and jinja2) may be confused, because standalone YAML does not come with a feature to concatenate variables.
However, we introduced a simple variable parser that replaces ``{{vars}}`` with their values. So far it is only possible to replace with simple vars, no list/dict items. E.g:
```yaml
mail: my@email.de
text: 'contact me at {{mail}}'
```
will lead to the desired result.

## Markdown
You can use Githubs markdown ([showdown](https://github.com/showdownjs/showdown) stricly speaking) when encapsulated in a div with the ``markdown`` class:
```html
<div class="markdown">
#this is a markdown page
we can use 
 * lists, 
 * _tables_, 
 * **formatting**
 * ...
here
</div>
```
For a detailed markdown description see the [showdown wiki syntax page](https://github.com/showdownjs/showdown/wiki/Showdown's-Markdown-syntax).

## placing Jinja2 templates outside of the pages/ directory
Sometimes it is useful to have other files as an template, too (e.g. for an .htaccess). Create a list ``root_templates`` in the config.yml and fill it with filenames relative to the document root (~/.public_html/). See the example project (``publish --init ~/projectpath``) for an explicit demonstration.

## copying static files with publish
Binaries and non-template files could in general directly placed into ``~/.public_html`` without informing ``publish`` about it. However, one might want to organize its template files within a version control system and/or want the whole webpage (including binarie content e.g. pdf downloads) reproduced by executing ``publish -o ~/.public_html`` while ~/.public_html is empty.  
To achive this fill the ``copy_files`` list with static files/dirs relative to the template root dir (where the config.yml is located at):
```yaml
copy_files:
    - img
    - pdf/announcements.pdf
```

## Subpages
At the moment it is not possible to have foldable sub categories in the navigation. We`ll investigate in some, if there are enough requests.
