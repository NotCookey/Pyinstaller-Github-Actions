# Pyinstaller-Github-Actions

## Overview

Pyinstaller-Github-Actions is a GitHub Actions automation script that simplifies the process of compiling your Python packages into cross-platform executables for Linux, macOS, and Windows. This enables you to create standalone executables from your Python code, making it easier for users to run your applications.

## Usage

To use this GitHub Actions workflow, follow these steps:

1. **Copy the Workflow**

    - Copy the following GitHub Actions YAML workflow and paste it into a `.github/workflows` directory within your repository. Save it as a `.yml` file (e.g., `build-executables.yml`).

```yaml
name: Build Executables

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        os: ['windows-latest', 'ubuntu-latest', 'macos-latest']

    env:
      MAIN_PY_FILE: 'main.py'  # Define the path to your main.py file here

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: 3.10.x

    - name: Install Python dependencies
      run: |
        pip install -r requirements.txt
      working-directory: ./

    - name: Install PyInstaller
      run: |
        pip install pyinstaller
      working-directory: ./

    - name: Build executable
      run: |
        pyinstaller ${{ env.MAIN_PY_FILE }}
      working-directory: ./

    - name: Create Artifact (Windows)
      if: matrix.os == 'windows-latest'
      uses: actions/upload-artifact@v3
      with:
        name: windows-executables
        path: dist/

    - name: Create Artifact (Linux)
      if: matrix.os == 'ubuntu-latest'
      uses: actions/upload-artifact@v3
      with:
        name: linux-executables
        path: dist/

    - name: Create Artifact (macOS)
      if: matrix.os == 'macos-latest'
      uses: actions/upload-artifact@v3
      with:
        name: macos-executables
        path: dist/

    - name: List files in dist folder
      run: ls -R ./dist/
```
