# Anchor Docs
 
## Building
 
### Getting the repo
 
If you are contributing to the repository, make sure to fork this repo, and then clone your own fork. You will need to do this to make creating pull requests easier.
 
```bash
git clone https://github.com/<your-username>/terra-docs
```
 
### Build locally with Docker
 
Using Docker is easier than configuring your local machine to build `sphinx`:
 
First, build the local Docker container:
 
```
make docker-make
```
 
Then you're ready to build the site:
 
```
make docker-build
```
 
You can view the site in your browser by navigating to `index.html` located in `<PATH_TO_CLONED_REPO>/_build/html/index.html`.
 
Make sure to run `make docker-build` each time you save your changes to view them live.
 
Alternatively, you can auto-rebuild the site and refresh the browser by using the following instead of `make docker-build `:
 
```
make docker-watch
```
 
## Creating pages
 
Pages can be written in markdown `.md` or restructured text `.rst`
 
Add new pages to the `/docs/` folder. Link pages to the navigation by inputting their relative filepath in the toctree located in `index.md`. Second-level pages can be made by adding a toctree to a parent file.
 
Markdown example:
 
To link a top-level page, insert a toctree and filepath into `index.md`:
 
```
:::{toctree}
:hidden:
/docs/top-level-page
/docs/other-top-level-page
:::
```
 
This will list top-level-page in the navigation.
 
To link a second-level page, add a filename's relative path to the toctree in the parent file.
 
In `top-level-page.md`, input the following toctree
 
```
:::{toctree}
:hidden:
/docs/second-level-page
:::
```
 
These two examples together will create the following navigation:
 
Top level page
- Second level page
Other top level page
 
### Links
 
All links written in markdown are relative. Full URLs will render as external filepaths.
 
### Admonitions (note and warning boxes)
 
Admonitions are made in markdown with the following syntax:
 
```
:::{warning}
 
This is the body of my warning admonition.
 
:::
```
 
Custom admonition syntax:
 
```
:::{admonition} This is my custom admonition title
:class: warning
 
This is the body of my admonition
 
:::
```
 
For a list of all admonition types, visit https://sphinx-book-theme.readthedocs.io/en/latest/reference/kitchen-sink/paragraph-markup.html?highlight=admonition#admonitions
 
 
### For more info on configuring, visit:
 
- [Sphinx Book Theme](https://sphinx-book-theme.readthedocs.io/en/stable/) for theme elements.
- [Myst parser](https://myst-parser.readthedocs.io/en/latest/index.html) for markdown syntax.
- [Sphinx-design](https://sphinx-design.readthedocs.io/en/sbt-theme/index.html) for tabs, cards, grids, dropdowns, and classes.
 
## Extensions
 
Extensions should be added to `requirements.txt` and `conf.py`.
 
## Redirects
 
Redirects are listed in `conf.py`. Visit https://documatt.gitlab.io/sphinx-reredirects/ for more info.
 
## Theme
 
Built using [Sphinx Book Theme](https://sphinx-book-theme.readthedocs.io/en/stable/). Visit https://sphinx-book-theme.readthedocs.io/en/latest/customize/custom-css.html for CSS customization.
 
 

