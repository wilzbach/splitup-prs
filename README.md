Split up PRs (alpha version)
============

Simple tool to split up changes of files in batches of PRs.
Note that this is for the very simple case if every change touches a separate file.

Support for splitting up more complicated PRs might come in the future.

How to use
----------

The tool assumes that you have forked the repo and cloned locally.
Moreover it can only commit non-committed changes, you might want to run `git reset --soft upstream/master`.

In the following it is assumed that you put this folder into your `$PATH` environment
variable or have created an `alias` to it.

Submitting step-by-step
-----------------------

The first command will show you all the files that are considered as changes.
Under the hood it parses `git status` and filters for files in `public/content`.
Note that to avoid merge conflicts the `index.yml` is ignored. If you start from scratch
you can safely submit the `index.yml` once all your major PRs were accepted.

```
splitup-prs
```

might print

```
Adding: public/content/de/welcome/links-documentation.md
Adding: public/content/de/welcome/run-d-program-locally.md
Simulation was run. Now use -f/--force to apply.
```

To submit these files to Github, execute

```
splitup-prs -f
```

This will use the CLI tool [`hub`](https://github.com/github/hub) to submit PRs
at Github and fallback to open a URL to submit the PR manually in case you haven't
installed the tool.

By default the tool will only process one PR per run, however once you are familiar
with the tool you can enable the batch mode with `-a` or `--all`.

```
splitup-prs -f --all
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
