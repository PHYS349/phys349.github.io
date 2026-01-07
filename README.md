# Instructor Guide: PHYS 349 Course Website

**Website:** [https://phys349.github.io/](https://phys349.github.io/)
**Repository:** [https://github.com/PHYS349/phys349.github.io](https://www.google.com/search?q=https://github.com/PHYS349/phys349.github.io)

This guide covers how to maintain, edit, and publish the PHYS 349 course website. The site is built using **Jupyter Book 2** and hosted via **GitHub Pages**.

---

## 1. Access & Permissions

**Host Account:**
The repository is currently hosted by **Jonathan Hood** (GitHub user: `jonhood11`).

* *Note:* This was created with a personal account rather than a Purdue University account to ensure easy access off-campus without strict 2FA/SSO blockers (a recommended practice for course sites).

**Transferring Control:**
To allow a new instructor or TA to edit the site, the current owner must either:

1. **Add them as a Collaborator:** Go to `Settings` -> `Collaborators` -> `Add people`.
2. **Transfer Ownership:** Go to `Settings` -> `General` -> `Transfer repository` (if handing over the course entirely).

---

## 2. Local Setup (First Time Only)

To run the website on your own computer, you need a Python environment. I recommend using VS Code and GitHub Desktop for file management.

### Step A: Install Python & Jupyter Book

1. Install **Anaconda** or **Miniconda**.
2. Open your terminal (or Anaconda Prompt) and create a new environment:
```shell
conda create -n jupyterbook python=3.11
conda activate jupyterbook

```


3. Install Jupyter Book 2 (ensure version is >= 2.0.0):
```shell
pip install "jupyter-book>=2.0.0"
pip install ghp-import

```



### Step B: Clone the Repository

1. Open **GitHub Desktop**.
2. Clone the repository `PHYS349/phys349.github.io` to your local machine.
3. Open this folder in **VS Code**.

---

## 3. Editing Content

The website content is controlled by **Markdown** (`.md`) files and **Jupyter Notebooks** (`.ipynb`).

### The Configuration File: `myst.yml`

In Jupyter Book 2, `myst.yml` replaces the old `_config.yml` and `_toc.yml`. This single file controls the website title, settings, and **Table of Contents (TOC)**.

**To add a new page:**

1. Create your file (e.g., `lectures/week2.md`).
2. Open `myst.yml`.
3. Add the file to the `toc` section so it appears in the sidebar:

```yaml
project:
  title: PHYS 349 - Intro to Quantum Computing
  toc:
    - file: intro.md
    - file: syllabus.md
    - title: Lectures
      children:
        - file: lectures/week1.ipynb
        - file: lectures/week2.md  # <--- New file added here

```

---

## 4. Previewing Locally

Before publishing, you should view the site on your own computer to check formatting.

1. Open your terminal in the repository folder.
2. Run the start command:
```shell
jupyter book start .

```


3. You will see output indicating the server has started:
```text
✨✨✨  Starting Book Theme  ✨✨✨
Server started on port 3000! 🥳 🎉
👉  http://localhost:3000  👈

```


4. Click the link (http://localhost:3000) to view your changes live. The site will auto-update as you save files.

---

## 5. Building a PDF

Jupyter Book 2 uses **Typst** (a modern, fast LaTeX alternative) to generate PDFs.

**Configuration:**
Ensure your `myst.yml` has the export settings:

```yaml
project:
  exports:
    - format: pdf
      template: typst
      output: phys349_notes.pdf

```

**To Build:**
Run this command in your terminal:

```shell
jupyter book build . --pdf

```

The PDF will be generated in the `_build` folder.

---

## 6. Publishing to the Web

Editing files on your computer does **not** automatically update the live website. You must push your changes to GitHub Pages.

### Method 1: The "ghp-import" magic (Recommended)

This command builds your HTML and pushes it to the special `gh-pages` branch that GitHub serves to the public.

1. Build the HTML version first:
```shell
jupyter book build .

```


2. Publish using `ghp-import`:
```shell
ghp-import -n -p -f _build/html

```


* `-n`: Includes a `.nojekyll` file (prevents GitHub from breaking formatting).
* `-p`: Pushes changes to GitHub.
* `-f`: Forces the update.



### Method 2: GitHub Actions (Automated)

*If you have a `.github/workflows/deploy.yml` file set up:*

1. Commit your changes in GitHub Desktop.
2. Push to `main`.
3. GitHub will automatically build and publish the site (takes ~2 minutes).

**Check status:** [https://github.com/PHYS349/phys349.github.io/actions](https://www.google.com/search?q=https://github.com/PHYS349/phys349.github.io/actions)