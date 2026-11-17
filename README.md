# Systems Learning

A shared Obsidian vault for studying systems programming through research, hands-on implementation, experimentation, and explanation.

The repository contains the curriculum, shared notes, resources, and code related to the learning track.

## Setup

### Prerequisites

Install:

- Git
- Obsidian
- a GitHub account with access to this repository

### Clone the repository

```bash
git clone https://github.com/Umoren/systems-learning.git
cd systems-learning
```

### Open it in Obsidian

Open Obsidian and choose:

**Open another vault → Open folder as vault**

Select the cloned `systems-learning` folder.

The repository itself is the Obsidian vault.

## Shared styling

The vault includes shared CSS snippets under:

```text
.obsidian/snippets/
```

To enable them:

1. Open **Settings**
2. Go to **Appearance**
3. Find **CSS snippets**
4. Reload snippets if necessary
5. Enable the available snippets

## Working with the vault

Before starting work:

```bash
git pull --rebase
```

After making changes:

```bash
git add .
git commit -m "Describe the changes"
git push
```

Avoid editing the same note at the same time where possible to reduce merge conflicts.

## Learning approach

The learning process follows:

```text
WRITE
  ↓
PREDICT
  ↓
RUN
  ↓
INSPECT
  ↓
EXPLAIN
```

Code exercises should be written by hand and understood line by line.

Research can use documentation, books, lectures, source code, articles, `man` pages, and other references.

The goal is to build strong mental models of how systems work and be able to explain them clearly.
