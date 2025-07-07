# git-pc

**Pretty Commit: a simple wrapper to make Git commit trailers easier.**

## Overview

`git-pc` is a lightweight Bash wrapper around `git commit` that helps you include structured metadata in your commits using [Git trailers](https://git-scm.com/docs/git-interpret-trailers). It streamlines the process of adding commit types, optional categories, and breaking change indicators without cluttering your commit summary.

## Features

- Specify commit type with flags like `--feat`, `--fix`, `--chore`, etc.
- Optionally provide a category for the type.
- Mark commits as breaking changes with `--breaking`.
- Pass all standard `git commit` options transparently.

## Installation

1. Clone this repository:

```bash
git clone https://github.com/heyvito/git-pc.git
cd git-pc
```

2. Copy `git-pc` to a directory in your `PATH`, for example:

```bash
sudo cp git-pc /usr/local/bin/
```

3. Ensure it is executable:

```bash
chmod +x /usr/local/bin/git-pc
```

Once installed, you can invoke it as:

```bash
git pc [options]
```

Git automatically resolves `git pc` to `git-pc` if the script is in your `PATH`.

## Usage

### Basic examples

Commit with a type:

```bash
git pc --feat -m "Add user login endpoint"
```

Commit with type and category:

```bash
git pc --feat auth -m "Support JWT tokens"
```

Commit marking a breaking change:

```bash
git pc --feat auth --breaking -m "Refactor token validation flow"
```

### Resulting commit trailers

Running:

```bash
git pc --feat "Tokenizer" --breaking "Tokenizer may do a backflip" -m "Allow tokenizer to do a backflip"
```

Produces a commit like:

```
Allow tokenizer to do a backflip

Type: feat
Scope: Tokenizer
Breaking: Tokenizer may do a backflip
```

Empty `breaking` arguments results in a `Breaking: true` trailer:

```bash
git pc --feat "Tokenizer" --breaking -m "Allow tokenizer to do a backflip"
```

Produces instead a commit like:

```
Allow tokenizer to do a backflip

Type: feat
Scope: Tokenizer
Breaking: true
```

Trailers can be parsed with:

```bash
git log -1 --pretty=format:"%B" | git interpret-trailers --parse
```

## Supported flags

- `--feat [scope]`
- `--fix [scope]`
- `--chore [scope]`
- `--build [scope]`
- `--ci [scope]`
- `--docs [scope]`
- `--style [scope]`
- `--refactor [scope]` or `--refac [scope]`
- `--perf [scope]`
- `--test` or `--spec [scope]`
- `--breaking [reason]`

Any other arguments are passed directly to `git commit`.

## Why?

Conventional commit prefixes in messages are useful for automated tools, but Git trailers provide a structured and parseable alternative without cluttering your commit summary. This script was inspired by [Sarah Mathey’s post](https://srazkvt.codeberg.page/posts/2025-07-06-conventional-commits-makes-me-sad.html) highlighting these ideas. A blog post about reasoning is also available [on my blog](https://vito.io/articles/2025-07-07-a-better-conventional-commit.html).

## Contributing

Contributions are welcome. Please open issues or pull requests with improvements or new features.

## License

```
MIT License

Copyright (c) 2025 - Vito Sartori

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

```
