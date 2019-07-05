# Farmshare Documentation

This is the Farmshare (version 2) documentation base. It will
be driven by [mkdocs-jekyll](https://www.github.com/vsoch/mkdocs-jekyll)
and for now is just a set of html exports and converted files.

**under development**

## Development

The Drupal site hosted at [Farmshare 2](https://srcc.stanford.edu/farmshare2) was first extracted 
manually via the web interface. The raw files are in [src](src)

Next, a [custom script](scripts/convert.py) was written to convert from html to 
markdown. First, we install dependencies:

```bash
$ pip install -r scripts/requirements.txt
```

And then run over the content, pointing at the input and output directories, 
respectively

```bash
$ mkdir -p docs
$ python scripts/convert.py src/*.html docs
```

There were a few MediaWiki linked pages that also needed conversion:

```bash
$ wget -O scripts/mw2md.py https://raw.githubusercontent.com/vsoch/mw2md.py/master/mw2md.py
```

```bash
python scripts/mw2md.py src/mobaxterm.txt > docs/mobaxterm.md
```

And finally, moving images to be relative to the docs folder.

```bash
$ cp -R src/img docs/img
```
