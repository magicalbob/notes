# python notes

## pip install

## pyproject.toml (modern standard) defines all dependencies:

	[project.dependencies] → installed by default for any user (pip install ideacli)

	[project.optional-dependencies] → named "extra sets" (e.g. dev for development tools)

### requirements.txt (old-style):

Is still common for locking exact versions, but is not required if you use pyproject.toml.

Install it with `pip install -r requirements.txt`

pip install as a developer (editable + dev dependencies):

	pip install -e ".[dev]"

pip install as an end user (normal install):

        pip install .

The . tells pip to install from the local folder. [dev] tells pip to install from the optional dev group (in project.optional-dependencies). You could define another optional group, say test, and enable it along with dev using:

	pip install -e ".[dev,test]"

## PyPI Publishing

### Preparation

#### 1. Set up your project structure:
Ensure your project follows standard Python packaging structure with:
* `pyproject.toml` (modern) or `setup.py` (traditional)
* README.md (for package documentation)
* LICENSE file
* Source code in a package directory

#### 2. Configure build system in pyproject.toml:
```toml
[build-system]
requires = ["setuptools>=64.0", "wheel"]
build-backend = "setuptools.build_meta"
```

#### 3. Install build tools:
```bash
pip install --upgrade pip build twine
```

### Building Distribution Packages
#### 1. Create distribution packages:
```bash
python -m build
```

This creates in the dist/ directory:
* A source distribution (.tar.gz)
* A wheel distribution (.whl)

#### 2. Check your package before uploading:
```bash
twine check dist/*
```

### Publishing to TestPyPI (recommended first step)
#### 1. Register on TestPyPI:
* Create an account at https://test.pypi.org/account/register/
* Generate an API token in account settings

#### 2. Upload to TestPyPI:
```bash
twine upload --repository-url https://test.pypi.org/legacy/ dist/*
```

When prompted:
* Username: __token__
* Password: your TestPyPI token (including pypi- prefix)

#### 3. Test installation from TestPyPI:
```bash
pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ your-package
```

### Publishing to PyPI
#### 1. Register on PyPI:

* Create an account at https://pypi.org/account/register/
* Generate an API token in account settings

#### 2. Upload to PyPI:
```bash
twine upload dist/*
```
When prompted:
* Username: __token__
* Password: your PyPI token (including pypi- prefix)

#### 3. Verify your package:
```bash
pip install your-package
```

### Version Updates
#### 1. Update version in pyproject.toml:
```toml
[project]
name = "your-package"
version = "0.1.2"  # Increment this for each new release
```

#### 2. Rebuild and upload:
```bash
# Remove old builds
rm -rf dist/

# Create new distribution packages
python -m build

# Upload to PyPI
twine upload dist/*
```

### Automating with GitHub Actions
Create .github/workflows/publish.yml:
```yaml
name: Publish to PyPI

on:
  release:
    types: [published]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.x'
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install build twine
    - name: Build and publish
      env:
        TWINE_USERNAME: __token__
        TWINE_PASSWORD: ${{ secrets.PYPI_API_TOKEN }}
      run: |
        python -m build
        twine upload dist/*
```
Add your PyPI token as a GitHub secret named PYPI_API_TOKEN.

### PyPI Package Maintenance
#### Check download statistics:

* View at https://pypistats.org/packages/your-package

#### Update package metadata:
* Description, classifiers, URLs in pyproject.toml
* README.md for detailed documentation

#### Mark as deprecated (if needed):
```toml
[project]
# Add "Development Status" classifier
classifiers = [
    "Development Status :: 7 - Inactive",
]
```

#### Yanking releases (for problematic versions):
```bash
twine upload --skip-existing --repository pypi dist/* --skip-existing
```
