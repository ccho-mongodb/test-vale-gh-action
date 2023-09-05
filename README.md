# Vale Styles and GitHub Action for the TDBX Team

This directory contains the current set of [vale](https://github.com/errata-ai/vale) rules 
that the Drivers and Connectors docs team uses to avoid style guide
errors in Pull Requests. These rules represent a subset of style guide
rules that the team should follow, and have been selected due to their
low rate of false-positives.

Other existing and new rules should be added when we determine that
they can produce sufficiently low instances of false-positives.


## Install

1.  To install the GitHub action into a repository, copy the vale configuration
file, ``vale.ini``, into the root directory of the docs repo on the ``main`` or
``master`` branch.

3.  Copy the vale rule files and GitHub action workflow file from the
``.github`` directory to the equivalent location in the root directory
of the docs repo.

After you perform the copy, the root directory of the docs repo should contain
the following file structure:

```
.vale.ini
.github
├── styles
│   └── MongoDB
│       ├── Abbreviations.yml
...
└── workflows
    └── vale-tdbx.yml
```

After you merge these changes into the ``main`` or ``master`` branch, the
action should automatically run whenever you commit new files.

## Known Issues

The GitHub action does not fail the check when vale reports errors.
While this should be controlled by the ``fail_on_error`` setting
it appears there is something wrong with the logic in the
reviewdog branch.


## Additional Information

This action currently depends on the following GitHub actions:

- [actions/checkout@master](https://github.com/actions/checkout)
- [masesgroup/retrieve-changed-files@v2](https://github.com/masesgroup/retrieve-changed-files/releases/tag/v2)
- [https://github.com/errata-ai/vale-action@reviewdog](https://github.com/errata-ai/vale-action)
