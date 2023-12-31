# Vale Styles and GitHub Action for the TDBX Team

This directory contains the current set of [vale](https://github.com/errata-ai/vale) rules 
that the Drivers and Connectors docs team uses to avoid style guide
errors in Pull Requests. These rules represent a subset of style guide
rules that the team should follow, and have been selected due to their
low rate of false-positives.

Other existing and new rules should be added when we determine that
they can produce sufficiently low instances of false-positives.

:warning: Changes between rulesets:
> Changes to the logic in rules must be made to both the set of rules
> contained in this directory structure as well as the original
> ones. However, the severity levels are intentionally modified in
> order to simplify the TDBX workflow.

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

[Example pull request](https://github.com/mongodb/docs-ecosystem/pull/902)

After you merge these changes into the ``main`` or ``master`` branch, the
action should automatically run whenever you commit new files. You can
verify that the action is added by clicking the **Actions** tab and
confirming that "vale-checks" appears under "All workflows" in the left
pane.

![Screenshot 2023-09-05 at 11 23 29 AM](https://github.com/ccho-mongodb/test-vale-gh-action/assets/52428683/5ccaddc1-515c-40ef-9ebc-6250c2eb1d9c)

If successfully added, this action runs on commits to forks, owned by
authorized contributors, of that docs repo.

## Check the Output

The output shows you the files you changed in the PR and any
rule violations that vale identified.

The GitHub action should fail and display a red "X" when vale reports
errors. To check the detailed results of the run, click the green checkmark or
red X next to the commit in the Github **Conversation** tab. Then, click the
**Details** link to view the log.

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

- The GitHub action has not been packaged yet. Therefore, any configuration
or rule updates need to be copied manually to each repository.

 
## Additional Information

This action currently depends on the following GitHub actions:

- [actions/checkout@master](https://github.com/actions/checkout)
- [masesgroup/retrieve-changed-files@v2](https://github.com/masesgroup/retrieve-changed-files/releases/tag/v2)
- [vale-action@reviewdog](https://github.com/errata-ai/vale-action)


# Install Vale Rules for the CLI

1. [Install vale]([https://vale.sh/docs/vale-cli/installation/](https://github.com/10gen/mongodb-vale/tree/main)).

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

The ``vale.ini`` file expects a relative file structure that
references the location of the ``yml`` rule files. If you
need to run **vale** from a configuration file in a different
location, make sure that it references the correct set of
rules.

If you run **vale** with other sets of rules, you can create
a separate alias that references a different **vale.ini** file.
