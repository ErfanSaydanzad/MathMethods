# Lecture Note Website

This folder contains a static website generated from the LaTeX lecture notes in the parent project.

## Recommended Editing Workflow

Treat the HTML files as generated output. For continuous revision:

1. Edit lecture content in `chapters/*.tex` in the parent project.
2. Put reusable JavaScript visualizations in `web/visualizations/`.
3. Put data files for visualizations in `web/data/`.
4. Rebuild the site:

```powershell
python tools\build_site.py
```

5. Check the output:

```powershell
python tools\check_site.py
```

6. Preview locally:

```powershell
.\tools\preview.ps1
```

Then open `http://localhost:8000`.

## Structure

- `index.html` is the homepage.
- `toc.html` is the full table of contents.
- `chapters/chapter-XX/index.html` contains one chapter per folder.
- `chapters/chapter-XX/assets/img/` contains figures copied or generated for that chapter. It is cleared and rewritten on every build.
- `assets/css/site.css` is the shared stylesheet.
- `assets/js/main.js` is the generated shared JavaScript file.
- `assets/js/visualizations/` and `assets/data/` are copied from `web/` in the parent project on every build. Edit the originals under `web/`, not these copies.

In the parent project:

- `tools/build_site.py` rebuilds the website from the LaTeX sources.
- `tools/check_site.py` checks for common conversion problems.
- `tools/figures/` generates the chapter figures with matplotlib.
- `templates/` contains copyable authoring examples for chapters, callouts, and interactive figures.

## Rebuilding

From the project root, run:

```powershell
python tools\build_site.py
```

The original LaTeX files are read but not modified. Existing generated website files may be overwritten.

Do not make long-term edits directly inside `chapters/chapter-XX/index.html`, `index.html`, `toc.html`, `assets/css/site.css`, or `assets/js/main.js`. Those files are generated.

## Adding Chapters

Add the chapter to `Lecture_Note.tex` using the existing `\chapwithfront{...}` and `\input{chapters/...}` pattern, then rebuild. The generator will create a new chapter folder and update navigation.

## Definitions, Theorems, Examples, and Solutions

Use consistent LaTeX environments when possible:

```latex
\begin{definition}[Name]
...
\end{definition}

\begin{example}
...
\end{example}

\begin{solutionbox}
...
\end{solutionbox}
```

This keeps the website boxes visually consistent. If an old example has `\textbf{Solution:}` inside the example, the generator tries to split it into a separate solution box, but explicit environments are more reliable.

## Interactive Figures

Create a placeholder in a LaTeX chapter with:

```latex
\webinteractive{fourier-demo}{Fourier Partial Sums}{Explore how adding terms changes the approximation.}
```

Then create a script whose filename matches the chapter slug, for example:

```text
assets/js/visualizations/chapter-07.js
```

Inside that file:

```js
window.LectureNotes.registerInteractiveFigure("fourier-demo", async (mount, LectureNotes) => {
  await LectureNotes.loadScript("https://cdn.plot.ly/plotly-2.35.2.min.js");
  mount.innerHTML = "";
  // Initialize Plotly, D3, p5.js, or custom JavaScript here.
});
```

The chapter script is loaded automatically on rebuild if it exists. Static images can remain in each chapter's `assets/img/` folder.
