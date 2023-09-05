# Vale Styles and GitHub Action for the TDBX Team

This directory contains the current set of [vale](https://github.com/errata-ai/vale) rules 
that the Drivers and Connectors docs team uses to avoid style guide
errors in Pull Requests. These rules represent a subset of style guide
rules that the team should follow, and have been selected due to their
low rate of false-positives.

Other existing and new rules should be added when we determine that
they can produce sufficiently low instances of false-positives.

Continue reading to see how to install the GitHub action and how to
install the rules for use with the CLI.

# GitHub Action

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

If successfully added, this action runs on commits to forks, owned by
authorized contributors, of that docs repo.

## Check the Output

The output shows you the files you changed in the PR and any
rule violations that vale identified.

Due to a known issue, the GitHub action does not fail when vale reports
errors. To check the results of the run, click the green checkmark next
to the commit in the Github **Conversation** tab. Then, click the **Details** 
link to view the log.

Alternatively, you can open a list of action logs from the  **Actions** tab
of the docs repo. You need to find and select the job that corresponds to the
one you recnetly committed to.

Click the **tdbx-vale** step of the job and then the arrow next to the
text "Running vale with reviewdog 🐶 ..."

An error should look something like this:

```
{"message": "[MongoDB.AvoidSubjunctive] Avoid the subjunctive 'should'.", "location": {"path": "source/whats-new.txt", "range": {"start": {"line": 73, "column": 63}}}, "severity": "ERROR"}
```

The ``message`` field identifies the rule that the file violates.

The ``path`` field identifies the file in which the error occurrs
and the ``range`` field identifies the specific part of the file.

Run your local copy of ``vale`` to verify the result.
Fix the errors and push a new commit to run the action again.

## Known Issues

- The GitHub action does not fail the check when vale reports errors.
While this should be controlled by the ``fail_on_error`` setting
it appears there is something wrong with the logic in the
reviewdog branch.

- The GitHub action has not been packaged yet. Therefore, any configuration
or rule updates need to be copied manually to each repository.

 
## Additional Information

This action currently depends on the following GitHub actions:

- [actions/checkout@master](https://github.com/actions/checkout)
- [masesgroup/retrieve-changed-files@v2](https://github.com/masesgroup/retrieve-changed-files/releases/tag/v2)
- [https://github.com/errata-ai/vale-action@reviewdog](https://github.com/errata-ai/vale-action)


# Install Vale Rules for the CLI

1. [Install vale](https://vale.sh/docs/vale-cli/installation/).

2. Install the ``vale`` rules and ``vale.ini`` configuration file.
The most convenient way to keep your **vale** CLI configured with
the same rules as this action is to fork and clone this repository,
and reference the file paths in your vale configuration.

One way to accomplish this is to create an alias that points to
your installation of ``vale`` and to pass it the location
of the ``vale.ini`` file contained in this repository. If you use
the GitHub repository file structure, you can declare the 
following alias in your ``zshrc``:

```bash
alias vale-tdbx="vale --config='/path/to/repo/tdbx/.vale.ini"
```

If you run **vale** with other sets of rules, you can create
a separate alias that references a different **vale.ini** file.
