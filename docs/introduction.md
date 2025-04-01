# Introduction

Most of my projects revolve around the versatility and power of Python, making it my go-to 
programming language for tackling a wide range of challenges. 

Whether it's automating workflows, building intelligent chatbots, or crafting efficient scripts, 
Python stands at the core of my work.


## Installation

### Virtual Environment

Create a virtual environment with:

=== "venv"

    ``` sh
    python -m venv .venv
    ```

=== "uv"

    ``` sh
    uv venv .venv # (1)!
    ```

    1.  `uv venv .venv` ensures that all virtual environments are created with the name `.venv`.
    To use the current directory name as the virtual environment's name, run:

        ``` sh
        uv venv
        ```  


Activate the virtual environment with:

=== "PowerShell"

    ``` powershell
    .\.venv\Scripts\activate
    ```

=== "Bash"

    ``` sh
    source .venv/Script/activate
    ```
Install dependencies with:

=== "uv"

    ``` sh
    uv pip install -r requirements.txt
    ```

=== "pip"

    ``` sh
    pip install -r requirements.txt 
    ```


When working on Python projects, it’s crucial to ensure that dependencies are 
consistent across different environments. A `requirements.txt` file helps achieve 
this by specifying the exact versions of packages used in your project.

=== "uv"

    ``` 
    uv pip freeze > requirements.txt
    ```

=== "pip"

    ``` 
    pip freeze > requirements.txt
    ```

