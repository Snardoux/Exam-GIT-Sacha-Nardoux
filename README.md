Campaign Validator is a small Python command-line application for checking advertising campaign data. It normalizes a campaign name, calculates a click-through-rate percentage, counts campaign tags, and verifies that the local campaign access token is configured correctly.

## Setup

This project requires Python 3.12 or newer and `uv`.

Install the project and its development dependencies:

uv sync

Run the application with:

uv run campaign-validator

Run the complete quality checks with:

uv run pytest

uv run mypy src

uv run ruff check .

uv run ruff format --check .

All commands should complete successfully before changes are committed.