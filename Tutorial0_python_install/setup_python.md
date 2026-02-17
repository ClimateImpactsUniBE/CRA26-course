
# Guide to set up Python Environments for Python using `uv`

> Author: Hugo Banderier, adapted from previous versions by Dani Steinfeld

`uv` is an open source *package* and *environment* management system for Python. It's a simple, straightforward and very robust command line tool, that you control via your terminal.

`uv` helps you to set up Python programming environments, in which you can develop and run applications. An environment is a directory, that contains the Python version and the packages that are completely compatible with your application. It is best practice to set up environments for each project. Why?

- Because you may need other versions of Python/packages for different applications and you want to make sure that older applications are still working.
- You are sharing your code or collaborating with someone else and you want to make sure that it is working on their computer ("reproducibility in science").

This why `uv` environments are best setup in a folder next to where your coding project is, and a new coding project should have its own different environment. None of these will be "global".

The following steps will help you with setting up `uv` and python environments on your personal computer. This will also install python itself

## Step 0: Using a terminal

MacOS and Linux share some basic components, and for all of this tutorial work the same way. Their terminal is simply called terminal, and all the commands to make it work are 99% the same.

In Windows, the terminal programme is called "Powershell", and the commands are a bit different from MacOS and Linux. The installation will also be a bit different but nothing to worry about. Fortunately all the basic commands are now the same. If you want to use Windows' old "terminal", we won't be able to help as much, and the commands are all different.

Doing anything in a terminal boils down to typing down commands with options and arguments, and pressing Enter. Your terminal is always *somewhere* in your computer, and what the commands do depends on where your terminal is. You can always check where you are using the command `pwd`.

The following small list of commands will be useful to begin with:
```shell
ls
```
will show you all the files and directory where the terminal is. On Windows' old terminal this was `dir`, and this syntax is valid in Powershell.
```shell
cd PATH
```
will move where the terminal is to `PATH`. It can be an absolute path (e.g. `cd /Users/hugo/Documents/code_local`) or relative to where you currently are (`cd code_local` if you are already in `/Users/hugo/Documents`).

Paths are written differently in Windows vs Mac/Linux. Separation is done with forward slashes in Mac/Linux (`/`) and backslashes in Windows (`\`), and the absolute paths always start with `/` in Mac/Linux while in Windows it starts with the disk name and a columns, e.g. `c:\windows\system32`. Otherwise it all works fairly similarly.

Other useful commands:
```shell
cp SOURCE DESTINATION
```
will copy-paste `SOURCE` to `DESTINATION`
```shell
mv SOURCE DESTINATION
```
will cut-paste, or move, `SOURCE` to `DESTINATION`. This is also how you can rename files and directories
```shell
rm FILE
```
will delete a file
```shell
rm -r DIR
```
will delete an empty directory or folder. 
```shell
rm -rf DIR
```
will delete a directory and all of its content. There is no recycle bin when you use rm, so any deletion is permanent. Be very careful.

The `-r` is a command's option, as you just saw they modify the behaviour of commands. They either come in long form with two dashes, in this case it would be `--recursive`, or in short form with a single dash, `-r`. Several short form options can be combined, `rm --recursive --force` is equivalent to `rm -r -f` is equivalent to `rm -rf`.

The list of options for a command can be obtained by looking up the manual for the command, `man COMMAND`, or the more condensed help page for the command, `COMMAND --help`. 
## Step 1: Install `uv`

For full instructions, visit <https://docs.astral.sh/uv/getting-started/installation/>

For everyone using Linux or MacOS, the following one-liner will work:
```shell 
curl -LsSf https://astral.sh/uv/install.sh | sh
```

On Windows Powershell, this should work:
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

You can check that it worked by trying to run the `uv` command anywhere.

## Extra: Uninstalling uv
If you want to fully uninstall uv. Don't do this now I guess.

Run the following commands

```bash
# Clean up stored data (optional):
uv cache clean
rm -r "$(uv python dir)"
rm -r "$(uv tool dir)"
# Remove the uv, uvx, and uvw binaries:
rm ~/.local/bin/uv ~/.local/bin/uvx
```

and for Windows:
```powershell
# Clean up stored data (optional):
uv cache clean
rm -r "$(uv python dir)"
rm -r "$(uv tool dir)"
# Remove the uv, uvx, and uvw binaries:
rm $HOME\.local\bin\uv.exe
rm $HOME\.local\bin\uvx.exe
rm $HOME\.local\bin\uvw.exe
```

## Step 2: Create an environment and install Python packages

Detailed information on how to manage environments can be found here:  <https://docs.astral.sh/uv/guides/projects/>

What you should to at this point is create a folder where your code (your python scripts and notebooks) will be. Mine will be located at the path `/Users/hugo/Documents/code_local/cra`. Go to that directory with your terminal (using `cd`) and there run `uv init`. Most of the work is already done, as this command installed python, created a barebones environment, and initialised a project. The directory should have this structure after this command is ran, and you can read about what all of this means in the link above. To check it looks like this you can run `ls -la`.

```
├── .venv
├── .python-version
├── README.md
├── main.py
├── pyproject.toml
└── uv.lock
```

Now python on its own is very useful, but you will save yourself a lot of time if you use someone else's code for some basic things you don't want to have to worry about. This is where packages come in. To install any package, say `numpy`, run the following command anywhere within your code directory:

```shell
uv add numpy
```
We will be using a few different packages throughout this course, they can all be added this way.

## Step 3 Working with notebooks

Jupyter notebooks are an alternative to python scripts that are extremely popular and convenient to use. This course is provided in the form of jupyter notebooks for this reason. To work with notebooks and not just scripts, one extra step is needed, and again it's just a few commands to run. First, the package `ipykernel` needs to be installed:

```shell
uv add ipykernel
```
And then the mouthful:
```shell
uv run ipython kernel install --user --env VIRTUAL_ENV $(pwd)/.venv --name=project
```

For windows:
```shell
uv run ipython kernel install --user --env VIRTUAL_ENV "$(PWD)\.venv" --name=project
```
You don't really need to understand this one, it just makes you *python environment* visible as a *IPython kernel*, a similar but different thing that jupyter needs to work. 
## Step 4: Decide on an IDE

An IDE (Integrated development environment) is a comprehensive text editor to write, test, debug and run your code in an easier way. Popular Python IDEs are:

- [VScode](https://code.visualstudio.com/)
- [Jupyterlab](https://jupyter.org/)
- [Pycharm](https://www.jetbrains.com/de-de/pycharm/)
- [Spyder](https://www.spyder-ide.org/)
- and many more ...

Which Python IDE is right for you? Only you can decide that :)  
Spyder has a similar interface to Rstudio and Matlab. Jupyterlab works on the browser and require no install. Personally, I use Visual Studio Code which needs some configuration (installing extensions) at the beginning, but now I also use it for documentation (markdown, LaTex). It can run python files, IPython code cells and Jupyter notebooks when the correct extensions are installed. Here, I will provide instructions for the first two, if you care so much about IDEs that you will go for something else I assume you know what you're doing. I cannot help with other IDEs.

### VScode (preferred way)

Download and install instructions can be found here <https://code.visualstudio.com/>. Once it is installed on your machine, you need to install a few extensions (you can look for them on the tab of the left by default, it's an icon with four squares). You need the "Python" extension as well as the "Jupyter" extension, both the official Microsoft ones. 

You should then "open file or folder" using the key combination vscode tells you to (on mac it's cmd + O, I guess it should be ctrl + O on other systems) and from there navigate to the folder you created in the previous section, for me `/Users/hugo/Documents/code_local/cra`.

Once you are there, with a python file or jupyter notebook open, you should be asked to "choose a python environment". You can choose the one uv created, it should be called `.venv` or something similar.

You can then start editing and running code from there. On the left column you have a nice file explorer, and at the bottom you have terminal where you can run the `uv` command for example.

VScode integrate LLM tools seamlessly, but as we wrote elsewhere we strongly recommend disabling these tools completely to help you actually learn. 

### Jupyterlab on the browser (next to no install)

Simply install jupyterlab as a package with `uv`:
```shell
uv add jupyterlab
```
and then run 
```shell
uv run --with jupyter jupyter lab
```
It will open a window on your default browser where you can directly edit and run code, nothing else to install.

## Step 5: What's next?

Now that you've set up your python programming environment, I recommend you play around with it. Best is to work trough some online tutorials and get familiar with different python packages, the IDE and conda.

Here are some great tutorials:

- Python:
  - for beginner (basic python): <https://docs.python.org/3/tutorial/>
  - Getting started with Python for Science, including packages numpy, scipy and matplotlib: <https://lectures.scientific-python.org/>
  - for more advanced Python user, I'll recommend the summer school at UZH: <https://www.physik.uzh.ch/~python/python_2021-06/index.php.html>
    - all lecture notes are online and can be found under 'Programme'  

- packages you should know (each with their own guide on how to install and get started):
  - [numpy](https://numpy.org/): creating and manipulating numerical data and multi-dimensional arrays
  - [scipy](https://www.scipy.org/): scientific computing
  - [polars](https://docs.pola.rs/api/python/stable/reference/index.html): The modern DataFrame library that everyone migrated to in the last couple of years. Extremely fast, great syntax. DataFrames are not ideal for geospatial data, but the best choice for tabular data.
  - [matplotlib](https://matplotlib.org/): for plotting
  - [cartopy](https://scitools.org.uk/cartopy/docs/latest/): for plotting on maps
  - [xarray](http://xarray.pydata.org/en/stable/): reading and manipulating netcdf data (build on top of numpy and pandas, very popular in the climate science community). Integrates with `dask` for high performance computing and lazy computations.
  - [scikit-learn](https://scikit-learn.org/stable/): statistical learning
  - [metpy](https://unidata.github.io/MetPy/latest/index.html): collection of functions for calculations with weather data
  - and many more!
    - you can even create your own packages and share them with others, for example the previous author of this tutorial wrote <https://github.com/steidani/ConTrack>

- [git](https://git-scm.com/) for version control:

    “Version control is a system that records changes to a file or set of
    files over time so that you can recall specific versions later.”  
    <https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control>

  - beginners git course of C2SM: <https://github.com/C2SM/git-course>
