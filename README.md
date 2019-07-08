# Farmshare Documentation

This is the Farmshare (version 2) documentation base. It is
driven by [mkdocs-jekyll](https://www.github.com/vsoch/mkdocs-jekyll).

<div style="width:50%;">
     <img src="assets/img/logo/farmshare-red-pot.png">
</div><br>

## Usage

### 1. Get the code

You can clone the repository:

```bash
git clone https://github.com/stanford-rc/docs-farmshare.git
```

### 2. Customize

Generally, you should see the the [getting started guide](https://vsoch.github.io/mkdocs-jekyll/getting-started)
for the template to better understand how to add custom elements, link to 
pages, create page tags, etc. A brief overview is provided here.


#### How do I add pages?

Documentation pages are found in [_docs/docs], and you can add them as links 
to other pages, or in the navigation directly by editing [_data/toc.myl](_data/toc.yml).
For more regular pages (e.g., "About") you can put them in the [pages](pages)
folder, and optionally add a `permalink` value to define the URL.

### How do I edit metadata?

To edit configuration values, customize the [_config.yml](_config.yml).
Most of the configuration values in the [_config.yml](_config.yml) are self explanatory,
and for more details, see the [about page](https://vsoch.github.io/mkdocs-jekyll/about/)
rendered on the site.

### How do I add alerts, boxes, and other elements?

See the [getting started guide](https://vsoch.github.io/mkdocs-jekyll/getting-started)
for the template.


### 3. Serve

Depending on how you installed jekyll:

```bash
jekyll serve
# or
bundle exec jekyll serve
```

## Development

The Drupal site hosted at [Farmshare 2](https://srcc.stanford.edu/farmshare2) was first extracted 
manually via the web interface. The raw files are in [src](src). First,
we needed to download the scripts to help wit conversion. There is one for
each of conversion from mediawiki, and from html.

```bash
$ wget -O scripts/mw2md.py https://raw.githubusercontent.com/vsoch/mw2md.py/master/mw2md.py
$ wget -O scripts/mw2md.py https://raw.githubusercontent.com/vsoch/mw2md.py/master/html2md.py
```

Install dependencies for the html conversion (markdownify):

```bash
$ pip install -r scripts/requirements.txt
```

Create the docs folder:

```bash
$ mkdir -p docs
```

And then convert the MediaWiki file:

```bash
$ python scripts/mw2md.py src/mobaxterm.txt > docs/mobaxterm.md
```

And then the html files:

```bash
for filename in $(ls src/*.html); do
    name=$(basename -- "$filename")
    name="${name%.*}"
    markdown="docs/${name}.md"
    if [ ! -f "${markdown}" ]; then
        echo "Converting $filename to $markdown"
        python scripts/html2md.py $filename $markdown
    fi
done
```

Note that after conversion, some manual work was done to fix tables (which
weren't parsed) and code blocks.

And finally, moving images to be relative to the docs folder.

```bash
$ cp -R src/img docs/img
```
