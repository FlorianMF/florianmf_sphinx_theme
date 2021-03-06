# FlorianMF Sphinx Theme

Sphinx theme for [PyTorch Docs](https://pytorch.readthedocs.io/en/latest/) based on the [Read the Docs Sphinx Theme](https://sphinx-rtd-theme.readthedocs.io/en/latest).

**This is an adapted version of the Pytorch Lightning sphinx theme.
See <https://sphinx-themes.org/> and <https://www.sphinx-doc.org/en/master/usage/theming.html> for other themes and tutorials on how to adapt them at your will.**

## Local Development

Run python setup:

```bash
python setup.py install
```

and install the dependencies using

```bash
pip install -r docs/requirements.txt
```

In the root directory install the `package.json`:

```bash
# node version 8.4.0
yarn install

```

If you have `npm` installed then run:

```bash
npm install
```

- If you want to see generated documentation for `docs/demo` then create
`.env.json` file and make it empty json file. Means `.env.json file` will
contain

```json
{}
```

Run grunt to build the html site and enable live reloading of the demo app at `localhost:1919`:

```bash
grunt
```

- If you want to specify the project folder (docs or tutorial for which
you want to see docs generated) then you need to specify it into `.env.json`
file:

```json
{
    "DOCS_DIR": "docs/",
    "TUTORIALS_DIR": "path/to/tutorial/directory"
}
```

Run grunt to build the html site for docs:

```bash
grunt --project=docs
```

and to build the html site for tutorial:

```bash
grunt --project=tutorials
```

The resulting site is a demo.

## Testing your changes and submitting a PR

When you are ready to submit a PR with your changes you can first test that your changes have been applied correctly against either the PyTorch Docs or Tutorials repo:

1. Run the `grunt build` task on your branch and commit the build to Github.
2. In your local docs or tutorials repo, remove any existing `florianmf_sphinx_theme` packages in the `src` folder (there should be a `pip-delete-this-directory.txt` file there)
3. In `requirements.txt` replace the existing git link with a link pointing to your commit or branch, e.g. `-e git+git://github.com/{ your repo }/florianmf_sphinx_theme.git@{ your commit hash }#egg=florianmf_sphinx_theme`
4. Install the requirements `pip install -r requirements.txt`
5. Remove the current build. In the docs this is `make clean`, tutorials is `make clean-cache`
6. Build the static site. In the docs this is `make html`, tutorials is `make html-noplot`
7. Open the site and look around. In the docs open `docs/build/html/index.html`, in the tutorials open `_build/html.index.html`

If your changes have been applied successfully, remove the build commit from your branch and submit your PR.

## Publishing the theme

Before the new changes are visible in the theme the maintainer will need to run the build process:

```bash
grunt build
```

Once that is successful commit the change to Github.

### Developing locally against PyTorch Docs and Tutorials

To be able to modify and preview the theme locally against the PyTorch  Docs and/or the PyTorch  Tutorials first clone the repositories:

- [PyTorch (Docs)](https://github.com/pytorch/pytorch)
- [PyTorch Tutorials](https://github.com/pytorch/tutorials)

Then follow the instructions in each repository to make the docs.

Once the docs have been successfully generated you should be able to run the following to create an html build.

#### Docs

```bash
# in ./docs
make html
```

#### Tutorials

```bash
# root directory
make html
```

Once these are successful, navigate to the `conf.py` file in each project. In the Docs these are at `./docs/source`. The Tutorials one can be found in the root directory.

In `conf.py` change the html theme to `florianmf_sphinx_theme` and point the html theme path to this repo's local folder, which will end up looking something like:

```python
html_theme = 'florianmf_sphinx_theme'
html_theme_path = ["../../../florianmf_sphinx_theme"]
```

Next create a file `.env.json` in the root of this repo with some keys/values referencing the local folders of the Docs and Tutorials repos:

```json
{
  "TUTORIALS_DIR": "../tutorials",
  "DOCS_DIR": "../pytorch_lightning/docs/source"
}

```

You can then build the Docs or Tutorials by running

```bash
grunt --project=docs
```

or

```bash
grunt --project=tutorials
```

These will generate a live-reloaded local build for the respective projects available at `localhost:1919`.

Note that while live reloading works these two projects are hefty and will take a few seconds to build and reload, especially the Docs.

### Built-in Stylesheets and Fonts

There are a couple of stylesheets and fonts inside the Docs and Tutorials repos themselves meant to override the existing theme. To ensure the most accurate styles we should comment out those files until the maintainers of those repos remove them:

#### Docs

```python
# ./docs/source/conf.py

html_context = {
    # 'css_files': [
    #     'https://fonts.googleapis.com/css?family=Lato',
    #     '_static/css/pytorch_theme.css'
    # ],
}
```

#### Tutorials

```python
# ./conf.py

# app.add_stylesheet('css/pytorch_theme.css')
# app.add_stylesheet('https://fonts.googleapis.com/css?family=Lato')
```

### Top/Mobile Navigation

The top navigation and mobile menu expect an "active" state for one of the menu items. To ensure that either "Docs" or "Tutorials" is marked as active, set the following config value in the respective `conf.py`, where `{project}` is either `"docs"` or `"tutorials"`.

```python
html_theme_options = {
  ...
  'pytorch_project': {project}
  ...
}
```

## Modifying the theme

In order to change the appearance of the html web sites you can change in the `theme.css`, for instance, the color of the links [here](florianmf_sphinx_theme/static/css/theme.css#L11361) or the colors of the left sidebar [here](florianmf_sphinx_theme/static/css/theme.css#L11084).

The general layout of the website can be modified in the [layout.html](florianmf_sphinx_theme/layout.html) file.

General configuration can be set in the [theme.conf](florianmf_sphinx_theme/theme.conf) file.

The links used as `jinja variables` should be overridden in the project which uses this sphinx theme.

Images and icons can be replaced [here](images) and fonts [here](fonts).

## Adapt it to your repo

You should replace all occurrences of:

- `https://github.com/../..` by the domain name of your git remote repository provider
- `FlorianMF` by your git repo account name
- `florianmf_sphinx_theme` by your repo name
