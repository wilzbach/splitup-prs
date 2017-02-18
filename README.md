Split up changes into PRs (alpha version)
=========================================

Simple tool to split up changes of files in batches of PRs.
Note that this is for the very simple case if every change touches a separate file.

Support for splitting up more complicated PRs might come in the future.

How to use
----------

The tool assumes that you have forked the repo you want to submit PRs too (e.g. dlang-tour) and cloned locally. Moreover this tool can only commit non-committed changes, so you might want to run `git reset --soft upstream/master`.

You can get the bash helper by downloading it directly from Github:

```sh
wget https://raw.githubusercontent.com/wilzbach/splitup-prs/master/splitup-prs
chmod +x splitup-prs
```

In the following it is assumed that you have created an `alias` (e.g. `alias splitup-prs="~/projects/dlang/splitup-prs/splitup-prs"` to it or have put this file into a folder of your `$PATH` environment (e.g. `/usr/local/bin`) variable.

Requirements
------------

- bash
- [hub][hub] (optional - needed to automatically create the PR)

Submitting step-by-step
-----------------------

The first command will show you all the files that are considered as changes.
Under the hood it parses `git status` and filters for files in `public/content`.
Note that to avoid merge conflicts the `index.yml` is ignored. If you start from scratch
you can safely submit the `index.yml` once all your major PRs were accepted.

```
splitup-prs
```

This might print:

```
Adding: public/content/de/welcome/links-documentation.md
Adding: public/content/de/welcome/run-d-program-locally.md
Simulation was run. Now use -f/--force to apply.
```

Accordingly to submit these files to Github, execute:

```
splitup-prs -f
```

This will use the CLI tool [`hub`][hub] to submit PRs at Github
and fallback to open a URL to submit the PR manually in case you haven't
installed the tool.

By default the tool will only process one PR per run, however once you are familiar
with the tool you can enable the batch mode with `-a` or `--all`.

```
splitup-prs -f --all
```

Using with an existing commit
-----------------------------

If you run this the first time, make a copy of your work or make a new clone of
the repository.

### A) With an commit on `master`

Simply reset your changes to the the ones of the `upstream` branch.
Warning: this will remove your new commits, but not the changes.

```
git reset --soft upstream/master
```

If you haven't added an `upstream` remote yet, you can also try to remove the commits directly.
For example to remove one commit, but keep the changes run:

```
git reset --soft HEAD~1
```

### B) With a custom branch

Change to your master branch and checkout all changes from your branch:

```
git checkout master
git checkout my-fancy-branch -- .
```

Adding an `upstream` remote endpoint
------------------------------------

If you don't use [`hub`][hub], you should to an upstream endpoint, s.t. the program
knows which URL to open:

```
git remote add origin git@github.com:dlang-tour/english.git
```

More features
-------------

### `-p`, `--prefix`

To categorize the PRs the `-p` or `--prefix` flag can be given. It will
be prefixed before every PR. Example:

```
splitup-prs -p "[german]"
```

Help
----

Let me know if you run into [issues](https://github.com/wilzbach/splitup-prs/issues).

License
-------

Boost License. See LICENSE.txt for more details.

[hub]: https://github.com/github/hub
