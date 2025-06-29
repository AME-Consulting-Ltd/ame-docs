# Contributing to Documentation

This guide explains how to contribute to AME's internal documentation using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/), a modern static site generator built on top of MkDocs.

---

## 1. Clone the Documentation Repository

Make sure you are a member of the [AME GitHub Organization](https://github.com/AME-Consulting-Ltd) and have access to the documentation repository.

```sh
git clone https://github.com/AME-Consulting-Ltd/ame-docs.git
cd ame-docs
```

---

## 2. Set Up Your Environment

Install Python (latest version) and then set up a virtual environment:

```sh
python -m venv venv
source venv/Scripts/activate  # Windows Git Bash
# or
venv\Scripts\activate.bat   # Command Prompt
```

Install MkDocs and the Material theme:

```sh
pip install mkdocs-material
```

Optionally, install live reload plugins:

```sh
pip install mkdocs-literate-nav mkdocs-glightbox
```

---

## 3. Preview the Docs Locally

From the root of the project, run:

```sh
mkdocs serve
```

Visit the local site at: [http://127.0.0.1:8000](http://127.0.0.1:8000)

---

## 4. Adding Content

All documentation pages are located inside the `docs/` directory.

- Use `.md` (Markdown) files for each page.
- Organize sections in folders like:
  - `docs/ame-timesheets/`
  - `docs/onboarding/`
  - `docs/how-to/`

Edit the navigation structure in `mkdocs.yml` to reflect new or moved pages.

Example:

```yaml
nav:
  - Home: index.md
  - Onboarding:
      - Environment Setup: onboarding/setup.md
```

---

## 5. Style & Formatting Guidelines

- Use level 1 headings (`#`) only for page titles
- Keep page titles consistent with `mkdocs.yml`
- Favor clarity over cleverness
- Use lists, bullet points, and code blocks where helpful
- Follow [Material for MkDocs syntax](https://squidfunk.github.io/mkdocs-material/reference/)

---

## 6. Building for Production

To generate a static site:

```sh
mkdocs build
```

The final site will be generated in the `site/` folder.

---

## 7. Deployment (Under Construction)

This site will be hosted via **GitHub Pages** once it's finalized.

In the future, updates will be pushed to the `gh-pages` branch via:

```sh
mkdocs gh-deploy
```

For now, deployment details are under construction.

---

## Helpful Resources

- MkDocs: https://www.mkdocs.org/
- Material for MkDocs: https://squidfunk.github.io/mkdocs-material/
- Markdown Cheatsheet: https://www.markdownguide.org/basic-syntax/

---

Please reach out to the maintainers if you have questions or want help making your first contribution.
