## Pre-commit
In practical project, we usually define some convention to allow other developers to follow on. It does not only ensure that error-prone code rarely happens but also make the product more prettier. Then pre-commit comes into the play

Whenever developers want to commit their code to the upstream branch, they have to make an assurance that the conventions are well-followed

With several steps of setting, you can make it happen

### Install the pre-commit
``
pip install pre-commit
``

### Add a pre-commit configuration in your project:
The file comes in the extension of `yaml` called `.pre-commit-config.yaml`. In this file, we set up something called `hook`. The hooks, for instance, can be commit, push, rebase. Below is the example of my settings

```
minimum_pre_commit_version: "2.17"  # it specifys the minimum version of the pre-commit 

#The language properties in the pre-commit behaves exactly like a installer that means whenever the hook's executable cannot found on the system subsequently the installer will have the reponsibility to download it.

default_language_version:
  python: python3

#With TRUE it will stop when any hook fails
fail_fast: false

# This is where we set up our hooks
repos:
    # to denote that this is from the local not the remote
  - repo: local
    hooks:
      - id: clang-format
        name: clang-format
        entry: clang-format -i  # entry will invoke the executable
        args: ["-fallback-style=none"]
        language: python
        files: |
          (?x)(
              ^src/.*
            )$
```

### Make the Clang for the Hook
In the above I made a hook for clang-format. Then we need to create a clang-format file to allow it to run on

```
    clang-format -style=LLVM -dump-config > .clang-format
```

This will create and place the format file in the current folder

---
**NOTE**

The already made file might have the format of UTF-16. This won't give us the allowance to run the pre-commit. So if this is the case, you need to tweak it to UTF-8 format

---



## Usage
Here are some ways that you can run pre-commit
```
    pre-commit run # This will automatically operate on git commit hook
```

```
    # We choose the hook and the commit to run on
    pre-commit run --hook-stage push --from-ref HEAD~1 --to-ref HEAD
```
