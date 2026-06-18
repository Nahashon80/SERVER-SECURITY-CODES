name: Python Security Guard

on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]

jobs:
  security-audit:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository code
      uses: actions/checkout@v4

    - name: Set up Python
      uses: actions/setup-python@v5
      with:
        python-node: '3.11'

    - name: Install security scanners
      run: |
        python -m pip install --upgrade pip
        pip install bandit safety

    - name: Run Bandit Static Analysis
      run: |
        # Scans project for hardcoded secrets and vulnerable code
        bandit -r . -v

    - name: Check Dependencies with Safety
      run: |
        # Creates a mock requirements file if none exists to prevent failure
        touch requirements.txt
        safety check -r requirements.txt
