# Python Linting
- Linting is the process of automatically checking your code for potential problems, mistakes, and style issues without actually running the program
    - Like a code reviewer that's continuously examining your code and pointing things out that you may have missed
- Can usually identify things like
    - Unused variables
    - Undefined variables
    - Missing imports
    - Potential bugs
    - Incorrect idententation or formatting
    - Code that is difficult to read or maintain
    - Violations of Python coding conventatins
- Several popular tools
    - Ruff
        - Recommended as it's faster and has the functionality of the others builtin
        - It can also handle formatting, which would have required a seperate tool
            - Formatters make your code consistantly readable
        - Installing it
            - With UV
                - `uv tool install ruff`
                    - Installs ruff globally so it's available across projects
        - Using it via the terminal
            - Checking code for errors and style issues
                -  `ruff check .`
            - Automatically fix issues where possible
                - `ruff check --fix .`
            - Format the code
                - `ruff format .`
            - Check formatting without changing the file
                - `ruff format --check .`
            - Checking a specific file
                - `ruff check file_name.py`
            - Formatting a specific file
                - `ruff format main.py`
        - The commands use the default settings, which are fine for most cases, but if we want to configure it, add RUFF details to the pyproject.toml file
            ```
            [tool.ruff]
            line-length = 88
            target-version = "py312"

            [tool.ruff.lint]
            select = ["E", "F", "I", "B", "UP"]

            [tool.ruff.format]
            quote-style = "double"
            ```
            - This is one example of what you could put, but each project could have it's own specific configs
            - So now the next time you run one of the CLI commands, Ruff checks this file and uses these configs instead of the default
        - Integrating it with VS Code so the checks happen automatically after you save a file
            - Install the Ruff extension by Astral
            - Open Settings (JSON)
                - Add the following
                    ```
                    {
                        "[python]": {
                            "editor.defaultFormatter": "charliermarsh.ruff",
                            "editor.formatOnSave": true
                        },
                        "ruff.lint.run": "onSave"
                    }
                    ```
    - Pylint
    - Flake8
    - Mypy
- If you're using VS code, it's Python extension comes with Pylance- which can do basic linting
    - But dedicated linters add further functionality by checking code quality, consistancy, and additional potential bugs