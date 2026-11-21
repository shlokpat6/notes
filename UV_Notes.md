# UV
- It's a Python package, project, and environment manager
- Takes several Python development tools and brings them into one fast workflow
    - Traditionally Python development involved several different tools
        - Python
            - Runs the Python code
        - pip
            - Installs packages
        - venv
            - Creates virtual environments
        - pipx
            - Installs standalone Python tools available globally
        - pyenv
            - Manages Python versions
        - pip-tools
            - Manages dependency versions
        - Poetry
            - Manages Python projects and dependencies
    - Traditional workflow - not difficult, but tedious
        - Install pyenv
            - Go to the their website and install it
        - Install a base python version
            - `pyenv install version_number`
        - Set the global python version
            - `pyenv global version_number`
        - Create a project directory
            - `mkdir directory_name`
        - Select your directory python version if needed
            - `pyenv local version_number`
        - Create a virtual environment
            - `python -m venv .venv`
        - Activate the virtual environment
            - `source .venv/Scripts/activate`
            - You'll have to do this everytime you want to work on your project again
        - Install dependencies
            - `pip install package_name`
        - Save your dependencies 
            - `pip freeze > requirements.txt`
        - Create the git repo
            - `git init`
        - deactivate your venv
            - `deactivate`
    - Installing UV- 2 main ways
        - Via pip
            pip install uv
            - The problem with this setup is that it installs UV within your global python installation
            - If or when you change that python installation, uv is destroyed
        - A standalone UV install (recommended)
            - In something like git bash, run
                - `powershell.exe -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
            - This installs UV outside the python installation, making it future proof
            - When it installs, you'll need to close the CLI and re-open it so that UV can get added to the path variable
            - Double check it was installed correctly
                which uv
                - Should give you the path
                - It should have installed it in a path that resembles this on windows
                    - `/c/Users/your_username/.local/bin/uv`
    - UV workflow
        - Create a uv project
            uv init project_name
            - This creates a project structure that looks like
                ```text
                my_project/
                ├── .git/
                ├── .gitignore
                ├── .python-version
                ├── README.md
                ├── pyproject.toml
                ├── uv.lock
                └── src/
                    └── my_project/
                        └── __init__.py
                ```
                - .venv
                    - This holds the local python environment
                    - The dependencies are installed here
                    - So just creating a directory in uv, initalized the venv
                        - It also automatically activates it
                            - You won't see the venv above the cli tag, because UV handles that behind the scenes
                        - It also automatically deactivates it, when you leave the project
                - pyproject.toml
                    - Your project's configuration and declared dependencies
                - uv.lock
                    - Records the exact dependency versions and other information needed to reproduce the environment
                - .python-version
                    - Specifies the Python version associated with the project, when it was configured
                - .gitignore
                    - List of files and directories that you don't want to be pushed up to github
                - .git/
                    - Holds all the git info
                - README.md
                    - A markdown file where we can put information about our project
                - src/my_project/`__init__.py`
                    - This uv structure is common for building packages
                    - src directory indicates that everything inside is actually code and not environment
                    - my_project is the name of your project
                    - `__init__.py` is a special Python file that tells Python that the directory should be treated as a Python package
                        - Not really needed in modern python anymore, as by default all python containing directories can be considred a package
                        - However, this file always runs whenever you import something from the project file
                            - So it's useful to have if you want some code to run like for setup before your modules are imported
            - The benefit to this structure is that when others clone your repository, all they have to do is run
                - `uv sync`
                    - UV will read the project configuration in pyproject.toml and the uv.lock file, creates the virtual environment, and installs the appropriate dependencies
                        - They don't have to manually do all those steps
                    - Assumes they have UV installed
            - UV also intialzies a git repo by default if you have git setup
                - It'll take your global config details
                - If you don't want a repo 
                   - `uv init --bare`
            - We could also get a flatter structure which older uv init used to do by running
                - `uv init --no-package`
                - Gives us
                    ```text
                    my_project/
                    ├── .git/
                    ├── .gitignore
                    ├── .python-version
                    ├── README.md
                    ├── pyproject.toml
                    ├── uv.lock
                    └── main.py
                    ```
                - This is the simpler option, and probably the one you'll use the most
        - Working with packages
            - Locally
                - Installing within a project
                    - `uv add package_name`
                - Checking which packages are installed
                    - `uv list`
                - Uninstalling tools
                    - `uv remove package_name`
            - Globally
                - Installing
                    - `uv tool install package_name`
                    - These would be available across projects
                - Checking which packages are installed
                    - `uv tool list`
                - Uninstalling a package
                    - `uv tool uninstall package_name`
        - Installing python versions
            - You can install different python version via
                - `uv python install python_version`
            - To see which versions are installed
                - `uv python list --only-installed`
            - To see which ones are available for download
                - `uv python list`
            - To choose a python version for your project directory
                - `uv init --python version_number`
                - Without the tag, uv will just use the currently selected global version
