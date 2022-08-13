# quarto_titlepages

This work is based on this section of the Quarto manual https://quarto.org/docs/journals/templates.html#replacing-partials
To see the LaTeX templates that Quarto is using start here: https://github.com/quarto-dev/quarto-cli/blob/main/src/resources/formats/pdf/pandoc/template.tex

This template makes a custom title page. The default document classes in Quarto are `scrbook` and `srcartcl`. There are some other classes in the `cls` folder (krantz, svmono, elsevier) and this works with those too.  Some of the title pages are inspired from [Latex Templates](http://www.latextemplates.com/cat/title-pages#google_vignette). Click [here](https://github.com/nmfs-opensci/quarto_titlepages/blob/e1384fabc59772a1211a693eca7b6490c68f9939/article.pdf) (or on `article.pdf` in repo) to see the output.

|                                    vline                                    | another one | and another |
|:---------------------------------------------:|:-----------:|:-----------:|
| ![](images/paste-8756BCE1.png){style="border: 5px solid #555;" width="90%"} | In Progress | In Progress |

## How to use

1.  Clone the repo (or grab all the files)
2.  Open one of two files and render

-   start with `article.qmd` for a single qmd document. Open it in RStudio and click Render.
-   NOT DONE YET: start with `_quarto.yml` for a Quarto project

## How it works

-   Defines titlepage (scrartcl) or frontmatter (scrbook) via a pandoc template in `partials/<name>/before-body.tex`.
-   Passes that template in via `template-partials`. This is needed so that you can reference the YAML variables, things like `author`.
-   Specifies the extra things (packages) that are needed for the LaTeX header in `partials/in-header.tex`.

## The YAML

    format:
      pdf:
        documentclass: scrartcl 
        number-sections: true
        template-partials: ["partials/vline/before-body.tex"]
        include-in-header: 
          - partials/vline/in-header.tex
        toc: true
        lof: true
        lot: true

## What is going on:

LaTeX document class affects the look;  `scrartcl` or `srcbook` are the Quarto defaults. The `cls` folder has a few more in it.

        documentclass: scrartcl

Articles generally don't have `#` (header 1) but instead just use `##` (header 2). If you use, `#` (header 1) in `scrartcl`, then you need to set

        number-sections: true 

so the numbering isn't whack.

This is the custom title page stuff. Change the directory to the titlepage you want. So like change `vline_article` to `vline_book`. You need to match `scrartcl` to `vline_article` and `scrbook` to `vline_book`.

        template-partials: ["partials/vline_article/before-body.tex"]
        include-in-header: 
          - partials/vline_book/in-header.tex

Next bit indicates if you want table of contents (toc), list of fig (lof), or list of tables (lot).

        toc: true
        lof: true
        lot: true

## Approach I took

There are 2 approaches that I considered.

1. Use the `title.tex` partial (which defines things like title and author) and then redefine the `\maketitle` command. Google and you'll find examples. I find `\maketitle` really irritating and is a constant headache to make custom title pages. But maybe you love it; in which case, try that approach.
2. Use `before-body.tex` partial to get rid of the `\maketitle` call and use my own `\begin{frontmatter}...\end{frontmatter}` section. I find this much more straightforward for creating custom titlepages. So that's the approach I took.
