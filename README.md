# Micropy Cli [![PyPI][pypi-img]][pypi-url] [![PyPI - Python Version][pypiv-img]][pypi-url] [![Travis (.com)][travis-img]][travis-url] [![Coverage Status][cover-img]][cover-url]


Micropy Cli is a project management/generation tool for writing [Micropython](https://micropython.org/) code in modern IDEs such as VSCode.
Its primary goal is to automate the process of creating a workspace complete with:

* **Linting** compatible with Micropython
* VSCode **Intellisense**
* **Autocompletion**
* Dependency Management (_WIP_)
* VCS Compatibility

<!-- Command line Application for automating Micropython project creation in Visual Studio Code. -->

<p align='center'>
    <img width='725' src='.github/img/micropy.svg' alt="Micropy Demo SVG">
</p>

[pypi-img]: https://img.shields.io/pypi/v/micropy-cli.svg?style=popout-square
[pypi-url]: https://pypi.org/project/micropy-cli/
[pypiv-img]: https://img.shields.io/pypi/pyversions/micropy-cli.svg?style=popout-square
[travis-img]: https://img.shields.io/travis/com/BradenM/micropy-cli/master.svg?style=popout-square
[travis-url]: https://travis-ci.com/BradenM/micropy-cli
[cover-img]: https://coveralls.io/repos/github/BradenM/micropy-cli/badge.svg
[cover-url]: https://coveralls.io/github/BradenM/micropy-cli

## Installation

You can download and install the latest version of this software from the Python package index (PyPI) as follows:

`pip install --upgrade micropy-cli`

## Usage

```sh
Usage: micropy [OPTIONS] COMMAND [ARGS]...

  CLI Application for creating/managing Micropython Projects.

Options:
  --version  Show the version and exit.
  --help     Show this message and exit.

Commands:
  init   Create new Micropython Project
  stubs  Manage Micropy Stubs
```

### Creating a Project

Creating a new project folder is as simple as:

1. Executing `micropy init <PROJECT NAME>`
2. Selecting your target device/firmware
3. Boom. Your workspace is ready.

#### Micropy Project Environment

When creating a project with `micropy-cli`, two special items are added:

* A `.micropy/` folder
* A `micropy.json` file

The `.micropy/` contains symlinks from your project to your `$HOME/.micropy/stubs` folder. By doing this, micropy can reference the required stub files for your project as relative to it, rather than using absolute paths to `$HOME/.micropy`. How does this benefit you? Thanks to this feature, you can feel free to push common setting files such as `settings.json` and `.pylint.rc` to your remote git repository. This way, others who clone your repo can achieve a matching workspace in their local environment.

> Note: The generated `.micropy/` folder should be *IGNORED* by your VCS. It is created locally for each environment via the `micropy.json` file.

The `micropy.json` file contains information micropy needs in order to resolve your projects required files when other clone your repo. Think of it as a `package.json` for micropython.

#### Cloning a Micropy Environment

To setup a Micropy environment locally, simply:

* Install `micropy-cli`
* Navigate to the project directory
* Execute `micropy`

Micropy will automatically configure and install any stubs required a project thanks to its `micropy.json` file.

### Stub Management

Stub files are the magic behind how micropy allows features such as linting, Intellisense, and autocompletion to work. To achieve the best results with MicropyCli, its important that you first add the appropriate stubs for the device/firmware your project uses.

> Note: When working in a micropy project, all stub related commands will also be executed on the active project. (i.e if in a project and you run `micropy stubs add <stub-name>`, then that stub retrieved AND added to the active project.)

#### Adding Stubs

Adding stubs to Micropy is a breeze. Simply run: `micropy stubs add <STUB_NAME>`
By sourcing [micropy-stubs](https://github.com/BradenM/micropy-stubs), MicroPy has several premade stub packages to choose from.

These packages generally use the following naming schema:

`<device>-<firmware>-<version>`

For example, running `micropy stubs add esp32-micropython-1.11.0` will install the following:
* Micropython Specific Stubs
* ESP32 Micropython v1.11 Device Specific Stubs
* Frozen Modules for both device and firmware

You can search stubs that are made available to Micropy via `micropy stubs search <QUERY>`

Alternatively, using `micropy stubs add <PATH>`, you can manually add stubs to Micropy.
For manual stub generation, please see [Josvel/micropython-stubber](https://github.com/Josverl/micropython-stubber).

#### Creating Stubs

Using `micropy stubs create <PORT/IP_ADDRESS>`, MicropyCli can automatically generate and add stubs from any Micropython device you have on hand. This can be done over both USB and WiFi.


#### Viewing Stubs

To list stubs you have installed, simply run `micropy stubs list`.

To search for stubs for your device, use `micropy stubs search <QUERY>`.


## Acknowledgements

### Micropython-Stubber
[Josvel/micropython-stubber](https://github.com/Josverl/micropython-stubber)

Josverl's Repo is full of information regarding Micropython compatibility with VSCode and more. To find out more about how this process works, take a look at it.

micropy-cli and [micropy-stubs](https://github.com/BradenM/micropy-stubs) depend on micropython-stubber for its ability to generate frozen modules, create stubs on a pyboard, and more.
