# Getting Started on Cloud Computing

This is the repository of a workshop/lesson "[Getting Started on Cloud Computing](https://pcbouman-eur.github.io/workshop-getting-started-cloud/)".
It uses the Jekyll styles and site setup of [The Carpentries](https://carpentries.org/) but is in no way officially affiliated with them.
This lessons builds on an alpha-version of the lesson on [Introduction to Using the Shell in a High-Performance Computing Context](http://www.hpc-carpentry.org/hpc-shell/).
But rather than a focus on HPC, it focuses on more private cloud computing, i.e. running your code on a typical AWS/Azure/Google Cloud Linux instance.
For that purpose, some things were changed:

* Added an episode with general history on computing an some explanation what the Cloud actually is
* Added an episode that explains how you typically set up a cloud instance, in the example Azure is used but general principles are discussed.
* Less focus on security when connecting to SSH. No RSA-keypairs are used in the workshop itself, this part is shortened and moved to a callout.
* Added three versions of an episode that discusses how to run your own programming code. The three versions are for three languages: Python, R and Java.
* Added a lesson that discusses `tmux`, so users can easily disconnect when their computations are running, and `cron`, in case they want to run something periodically (e.g. scraping)

[![Create a Slack Account with us](https://img.shields.io/badge/Create_Slack_Account-The_Carpentries-071159.svg)](https://swc-slack-invite.herokuapp.com/)

This repository generates the corresponding lesson website from [The Carpentries](https://carpentries.org/) repertoire of lessons. 

## Contributing

We welcome all contributions to improve the lesson! Maintainers will do their best to help you if you have any
questions, concerns, or experience any difficulties along the way.

We'd like to ask you to familiarize yourself with our [Contribution Guide](CONTRIBUTING.md) and have a look at
the [more detailed guidelines][lesson-example] on proper formatting, ways to render the lesson locally, and even
how to write new episodes.

Please see the current list of [issues][FIXME] for ideas for contributing to this
repository. For making your contribution, we use the GitHub flow, which is
nicely explained in the chapter [Contributing to a Project](http://git-scm.com/book/en/v2/GitHub-Contributing-to-a-Project) in Pro Git
by Scott Chacon.
Look for the tag ![good_first_issue](https://img.shields.io/badge/-good%20first%20issue-gold.svg). This indicates that the maintainers will welcome a pull request fixing this issue.  


## Maintainer(s)

Current maintainers of this lesson are 

* Paul Bouman


## Authors

For now, a list of some of the contributors to the lesson can be obtained from the
[commit history](https://github.com/pcbouman-eur/workshop-getting-started-cloud/graphs/contributors)

## Citation

To cite this lesson, please consult with [CITATION](CITATION)

[lesson-example]: https://carpentries.github.io/lesson-example
