Split up PRs (alpha version)
============

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

Submitting step-by-step
-----------------------

The first command will show you all the files that are considered as changes.
Under the hood it parses `git status` and filters for files in `public/content`.
Note that to avoid merge conflicts the `index.yml` is ignored. If you start from scratch
you can safely submit the `index.yml` once all your major PRs were accepted.
To categorize the PRs the `-p` or `--prefix` flag is required:

```
splitup-prs --prefix "[german]"
```

This might print:

```
Adding: public/content/de/welcome/links-documentation.md
Adding: public/content/de/welcome/run-d-program-locally.md
Simulation was run. Now use -f/--force to apply.
```

Accordingly to submit these files to Github, execute:

```
splitup-prs -p "[german]" -f
```

This will use the CLI tool [`hub`](https://github.com/github/hub) to submit PRs
at Github and fallback to open a URL to submit the PR manually in case you haven't
installed the tool.

By default the tool will only process one PR per run, however once you are familiar
with the tool you can enable the batch mode with `-a` or `--all`.

```
splitup-prs -p "[german]" -f --all
```

Help
----

Let me know if you run into [issues](https://github.com/wilzbach/splitup-prs/issues).

License
-------

Boost License. See LICENSE.txt for more details.
