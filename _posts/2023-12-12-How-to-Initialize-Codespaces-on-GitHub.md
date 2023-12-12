---
author: Raman Adlakha
bibliography: "D:.bib"
categories: note
image: bamboo_farm.jpg
layout: post
tags:
- github
- codespaces
- workspaces
title: How to Initialize Codespaces on GitHub
---

To use Codespaces, follow the steps below:

1.  Create a repo:

     Using the web interface of GitHub, create a new repository.
    Initialize a README.md file, a .gitignore file, and select a
    license.

2.  Launch Codespaces:

     From the web interface, launch Codespaces for the repository.

3.  Choose VS Code Desktop (optional):

     If you have VS Code installed on your desktop, you can choose to
    use it instead of the VS Code IDE in the browser. To switch to your
    desktop, click the hamburger button on the top left and select “Open
    in VS Code Desktop” or “Open in VS Code Insiders Desktop” as
    appropriate.

4.  Check status in VS Code Desktop:

     Once you open VS Code Desktop, switch to the terminal and issue the
    command `git status` to ensure everything is in order.

5.  Connect to Codespace from VS Code Desktop:

     In future sessions, you can start with your VS Code Desktop and
    connect to your Codespace using the command palette (CTRL+SHIFT+P)
    and selecting “Codespaces: Connect to codespace…”.

6.  Create a Python Virtual Environment:

     To create a Python virtual environment, follow these steps:

    1.  In the terminal tab of your VS Code, issue the
        command `python3 -m venv ~/.venv`.
    2.  Source the virtual environment every time a new terminal session
        is started by adding the source command to .bashrc for quick
        start. Edit your .bashrc file by running `code ~/.bashrc` and
        add `source ~/.venv/bin/activate` on a new line at the end of
        the file. Save and close the file. Optionally, while you are
        here, you can also add aliases for common command options for
        your work mode, such as `alias ll='ls -alF'`.
    3.  Activate the new Python environment by sourcing it or simply
        sourcing the edited .bashrc file. You can
        run `source ~/.bashrc` or `. ~/.bashrc`.

7.  Initialize requirements.txt:

    Initialize the `requirements.txt` file with all the libraries you
    require in your code. After downloading, installing, and referencing
    the packages in your code, use the
    command `pip freeze > requirements.txt`. If you are working on code
    by someone else and there is no `requirements.txt`, you can use the
    following steps:

    ``` bash
    pip install pipreqs
    pipreqs /path/to/your/project
    ```

I wrote this as a quick reminder to myself. Hope it helps you too. There
will be more small how-tos added here as I write them.
