# Daisy Documentation
This documentation is build by Jupyter Book. 

### Build the book’s HTML
The book's content is put in book folder and the book's structure is defined in `_toc.yml`.

By running the following command can build the HTML for the book.:

```bash
jupyter-book build mybookname/
```

This will generate a fully-functioning HTML site using a **static site generator**.
The site will be placed in the `_build/html` folder, something like this:

```bash
mybookname
 └──_build
    └── html
       ├── _images
       ├── _static
       ├── index.html
       ├── intro.html
       ...
```

These are the static files for a standalone website!
They contain the HTML and all assets needed to view your book in a browser.

You can open the pages in the site by navigating to that folder and opening the `html` files with your web browser.

:::{note}
You can also use the short-hand `jb` for `jupyter-book`. E.g.,:
`jb build mybookname/`.
:::


## Publish the book online with GitHub Pages

We have just pushed the *source files* for our book into our GitHub repository.
This makes it publicly accessible for you or others to see.

Next, we'll publish the *build artifact* of our book online, so that it is rendered as a website.


The easiest way to use GitHub Pages with your built HTML is to use the [`ghp-import`](https://github.com/davisp/ghp-import) package. `ghp-import` is a lightweight Python package that makes it easy to push HTML content to a GitHub repository.

`ghp-import` works by copying *all* of the contents of your built book (i.e., the `_build/html` folder) to a branch of your repository called `gh-pages`, and pushes it to GitHub. The `gh-pages` branch will be created and populated automatically for you by `ghp-import`. To use `ghp-import` to host your book online with GitHub Pages follow the steps below:

```{note}
Before performing the below steps, ensure that HTML has been built for each page of your book
(see {doc}`the previous section <../start/build>`). There should be a collection of HTML
files in your book's `_build/html` folder.
```

1. Install `ghp-import`

   ```bash
   pip install ghp-import
   ```
2. Update the settings for your GitHub pages site:

    a. Use the `gh-pages` branch to host your website.

    b. Choose root directory `/` if you're building the book in it's own repository.
       Choose `/docs` directory if you're building documentation with jupyter-book.

3. From the `main` branch of your book's root directory (which should contain the `_build/html` folder) call `ghp-import` and point it to your HTML files, like so:

   ```bash
   ghp-import -n -p -f _build/html
   ```

```{warning}
Make sure that you included the `-n` - this tells GitHub *not* to build your book with
[Jekyll](https://jekyllrb.com/), which we don't want because our HTML is already built!
If you do not do this you may see **404 not found** for your deployed content.
```

Typically after a few minutes your site should be viewable online at a url such as: `https://<user>.github.io/<myonlinebook>/`. If not, check your repository settings under **Options** -> **GitHub Pages** to ensure that the `gh-pages` branch is configured as the build source for GitHub Pages and/or to find the url address GitHub is building for you.

To update your online book, make changes to your book's content on the `main` branch of your repository, re-build your book with `jupyter-book build mybookname/` and then use `ghp-import -n -p -f mylocalbook/_build/html` as before to push the newly built HTML to the `gh-pages` branch.

```{warning}
Note this warning from the [`ghp-import` GitHub repository](https://github.com/davisp/ghp-import):
"...*`ghp-import` will DESTROY your gh-pages branch... and assumes that the `gh-pages` branch is 100% derivative. You should never edit files in your `gh-pages` branch by hand if you're using this script...*"
```
