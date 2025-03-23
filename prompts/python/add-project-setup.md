---
description: Coding Standards & Rules for Python Project Setup 2025 
globs: requirements.txt, **/pyproject.toml, **/Pipfile, **/setup.cfg, */environment.ini 
alwaysApply: false
---

# Python Project Setup (2025)

## Virtual Environments & Isolation

- Always create a dedicated virtual environment (using `python -m venv` or `virtualenv`) for each project to isolate dependencies. Do not rely on system-wide packages.
- Use a consistent directory (commonly named `.venv/`) for the virtual environment, and add it to `.gitignore` so it's never committed. Maintain the same structure across dev, CI, and prod for consistency.
- Upgrade pip and wheel inside the virtualenv regularly (`pip install --upgrade pip wheel`) to ensure the latest packaging security fixes and features are available.
- Avoid mixing dependency management tools in one project. For example, if using venv + pip with a `requirements.txt`, do not simultaneously use Pipenv or Poetry in the same project – pick one approach and stick to it.

## Dependency Management & Requirements

- Pin exact versions for all dependencies in `requirements.txt` (e.g., `package==1.2.3`) to ensure reproducible installs. Include transitive dependencies with locked versions if using a requirements freeze or lock file.
- Maintain a `requirements.txt` (and optionally a separate `requirements-dev.txt` for development/test packages) and keep it updated. When adding or upgrading packages, update this file using `pip freeze > requirements.txt` or a tool like pip-compile to capture exact versions and hashes for integrity.
- If using a modern packaging standard (PEP 621) with a `pyproject.toml` (and possibly `poetry.lock` or similar), ensure all dependencies are listed there instead of a requirements file. Don't duplicate dependency definitions in multiple places. Commit the lock file (Poetry's or pip-compile's output) for deterministic builds.
- Periodically review and update dependencies to latest patch/minor versions for security and bug fixes. Use `pip list --outdated` or automated tools (Dependabot, pip-audit, safety) to identify packages needing updates or with known vulnerabilities, and upgrade them in a controlled manner.

## Python Version Management

- Use a consistent Python version across all environments (development, CI, production) to avoid compatibility issues. Prefer the latest stable Python 3 release that is supported by your frameworks (e.g., Python 3.11+ in 2025) for performance and security improvements.
- Document the required Python version (for example, in a `runtime.txt`, `pyproject.toml` classifiers, or README). Optionally use tools like pyenv and a `.python-version` file to lock the Python interpreter version for the project.
- Avoid using Python releases beyond end-of-life. For instance, by 2025 Python 3.7 and 3.8 are end-of-life; use 3.10+ or above. Plan upgrades proactively when your Python version is nearing EOL to receive continued security patches.
- When deploying or containerizing, use an official Python base image that matches your version and pin it (e.g., `python:3.12-slim`). This ensures the environment is consistent and prevents drifting to an unintended Python patch version.

## Consistency & Security Best Practices

- Keep the development environment isolated: do not install project dependencies globally. If automation scripts are needed, consider a Makefile or scripts that activate the venv and install requirements, to standardize setup for all developers.
- Ensure the `requirements.txt` (or lock file) is the single source of truth for dependencies. New installations should come from it (e.g., `pip install -r requirements.txt`) rather than ad-hoc `pip install` commands to prevent environment drift.
- Use environment variables for configuration and secrets. Never commit sensitive data (API keys, database URLs, etc.) in settings or code. Provide a sample environment file (like `.env.example`) to document required vars. Load these variables in your application using a library or through the OS, not by hardcoding values.
- For supply-chain security, consider using `pip install --require-hashes -r requirements.txt` (if you've generated hashes via pip-compile) to verify package integrity. Install dependencies from trusted sources (PyPI or a vetted internal index) and avoid installing directly from arbitrary GitHub repositories or URLs in requirements unless necessary and reviewed.
- Regularly clean and rebuild your virtual environment (e.g., on CI or when updating many packages) to catch any missing dependencies or dev-only packages. This helps double-check that your `requirements.txt` or `pyproject.toml` accurately reflects all needed libraries.