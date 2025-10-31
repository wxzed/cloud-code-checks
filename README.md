
# GitHub Cloud CI

A repository for GitHub Actions workflow configuration for cloud-based code checking and CI/CD.

## Introduction

This repository provides a complete GitHub Actions workflow configuration for code checking, static analysis, and version verification on cloud virtual machines. Supports cloud checking for Python and C++ code.

## Features

- ✅ Support cloud-based virtual machine static compilation checks for C++ code (clang-tidy)
- ✅ Support cloud-based virtual machine static format checks for Python code (pylint)
- ✅ Support automatic tag version checking in the cloud
- ✅ Support automatic environment caching for faster builds
- ✅ Support triggers for Pull Request, Push, and Tags

## Usage

### 1. Copy Configuration Files

Copy the `.github` folder to your project root directory.

### 2. Configure Triggers

Modify the trigger conditions in `.github/workflows/ci.yml` according to your needs:

```yaml
on: 
  push:
    branches:
      - "**"       # Run on all branch pushes
  pull_request:    # Run on PR
  tags:
      - "v*.*.*"   # Run on tag creation
```

### 3. Push Code

Push your code to GitHub, and the workflow will trigger automatically:

```bash
git add .
git commit -m "Add CI workflow"
git push
```

### 4. View Results

Check the workflow execution results in the "Actions" tab of your GitHub repository.

## Included Check Tasks

### 1. pylint (Python Static Checking)

Checks all Python files to verify code standards and potential bugs.

- Configuration: `.github/config/.pylintrc`
- Triggers: push to branch, pull_request, repository_dispatch, workflow_call

### 2. clang-tidy (C++ Static Checking)

Uses PlatformIO to compile the project and generate `compile_commands.json`, then performs static analysis with clang-tidy.

- Configuration: `.github/config/.clang-tidy`
- Compile config: `.github/config/platformio.ini`
- Triggers: push to branch, pull_request, repository_dispatch, workflow_call

### 3. release (Version Checking)

Verifies that the tag version matches the version in `library.properties`.

- Triggers: Creating a version tag starting with `v` or `V`
- Permissions: Requires `contents: write` permission

## Workflow Structure

```
common      - Prepare common environment (Python, cache, etc.)
├── pylint  - Python code checking
├── clang-tidy - C++ code checking
├── release - Version verification (only on tags)
└── ci-status - Summarize all check results
```

## Configuration Files

All configuration files are located in `.github/config/`:

- `requirements.txt`: Python dependencies (pylint, etc.)
- `.pylintrc`: Python pylint checking configuration
- `.clang-tidy`: C++ clang-tidy checking configuration
- `platformio.ini`: PlatformIO compilation configuration (for generating compile_commands.json)

## Customization

### Modify Python Version

In `.github/workflows/ci.yml`:

```yaml
env:
  PYTHON_3: "3.11.13"  # Change to your desired version
```

### Modify Check Rules

- Python: Edit `.github/config/.pylintrc`
- C++: Edit `.github/config/.clang-tidy`

### Disable Certain Checks

In `.github/workflows/ci.yml`, you can comment out unneeded jobs or add conditions:

```yaml
pylint:
  if: false  # Disable pylint
```

## Notes

- Ensure your repository has corresponding source files (.py or .cpp/.h/.ino)
- If the project doesn't need C++ checking, you can remove the `clang-tidy` job and related dependencies
- Version checking requires a `library.properties` file in the project root
- The first run may take longer to download and install dependencies

