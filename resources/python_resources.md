# Python Resources

While prior experience with Python is **not required** for this course, it is a valuable tool for data analysis and scientific computing. If you are new to programming or need a refresher, the resources below will help you get started.

## 1. Recommended Setup for This Course

For this course, I recommend installing **Anaconda** and using **Jupyter Notebook** to write and run your code. This is the same environment we will use for in-class demonstrations and homework assignments.  

### Step 1: Install Anaconda

Download and install the [Anaconda Distribution](https://www.anaconda.com/download). Anaconda is a large download that includes Python along with hundreds of popular scientific computing packages (NumPy, Matplotlib, SciPy, etc.) and a graphical interface called Anaconda Navigator.

- Follow the installation instructions for your operating system (Windows, macOS, or Linux).
- Accept the default settings during installation.

### Step 2: Launch Jupyter Notebook

After installation, you have two options to launch Jupyter Notebook:

**Option A: Using Anaconda Navigator (Recommended for Beginners)**
1. Open Anaconda Navigator from your applications menu.
2. Find "Jupyter Notebook" and click **Launch**.
3. A new browser window will open with the Jupyter file browser.

**Option B: Using the Terminal/Command Line**
1. Open a terminal (macOS/Linux) or Anaconda Prompt (Windows).
2. Type `jupyter notebook` and press Enter.
3. A new browser window will open with the Jupyter file browser.

### Step 3: Verify Your Installation

To confirm everything is working:
1. In the Jupyter file browser, click **New** → **Python 3** to create a new notebook.
2. In the first cell, type:
```python
   import numpy as np
   print("Hello, PHYS 349!")
```
3. Press **Shift + Enter** to run the cell. If you see the message printed below the cell, your installation is complete.
```{note}
Jupyter Notebook runs in your web browser, but it is running Python locally on your computer. Your files are saved to your hard drive, not the cloud.
```

## 2. Alternative: Cloud-Based Python (No Installation Required)

If you have trouble installing Anaconda or prefer not to install software, you can use a cloud-based Jupyter environment. These run Python in your web browser with no installation required.

* **[Google Colab](https://colab.research.google.com/)**
    * Requires a Google account. Works like a Google Doc but for code—you can save notebooks directly to Google Drive.
    * Best for quick prototyping and access to free GPU resources.

* **[NanoHUB](https://nanohub.org/)**
    * Launch a standard Jupyter Notebook environment directly in your browser.
    * Widely used in the scientific community for reproducible research.
```{warning}
If you use a cloud-based environment, be aware that the interface may differ slightly from the local Jupyter Notebook we use in class. You are also responsible for ensuring your files are saved properly before submission.
```

## 3. Alternative Editors and IDEs

Once you are comfortable with Python, you may want to explore other development environments. An **IDE** (Integrated Development Environment) provides additional features like code completion, debugging tools, and project management.

* **[Visual Studio Code](https://code.visualstudio.com/)** (VS Code)
    * A free, lightweight editor from Microsoft. After installing, add the [Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python) to enable Python support.
    * VS Code can also open and run Jupyter notebooks directly.

* **[JupyterLab](https://jupyterlab.readthedocs.io/)**
    * The next-generation interface for Jupyter. It is included with Anaconda and offers a more powerful, multi-tabbed workspace compared to the classic Jupyter Notebook.
    * Launch it from Anaconda Navigator or by typing `jupyter lab` in the terminal.

* **Other options:** [PyCharm](https://www.jetbrains.com/pycharm/), [Spyder](https://www.spyder-ide.org/) (included with Anaconda)

## 4. Tutorials and Learning Materials

If you are new to Python, these free resources will help you get started:

**Interactive Beginner Tutorials**
* [W3Schools Python](https://www.w3schools.com/python/): Great for quick reference and simple "try it yourself" examples.
* [The Official Python Tutorial](https://docs.python.org/3/tutorial/index.html): The authoritative source, though it can be dense for absolute beginners.
* LLM's are a great tutors as well.  Try Claude, Gemini, and ChatGPT. 

**For Science and Engineering**
* [A Whirlwind Tour of Python](https://jakevdp.github.io/WhirlwindTourOfPython/): A fast-paced introduction designed for researchers who already understand basic programming logic.
* [SciPy Lecture Notes](https://scipy-lectures.org/): Tutorials on the scientific Python ecosystem (NumPy, Matplotlib, SciPy).

