---
description: A guide on how users and developers can contribute to the project.
cover: >-
  https://images.unsplash.com/photo-1571171637578-41bc2dd41cd2?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw5fHxzb2Z0d2FyZSUyMGRldmVsb3BtZW50fGVufDB8fHx8MTcwOTUwMTc5MXww&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Contributing to catch22

While the _catch22_ feature set is fixed and we're not seeking to expand it with new features, we highly value contributions that enhance the existing framework. For example, we welcome improvements aimed at increasing the efficiency of existing time series feature calculations, such as optimising code to run faster (on your preferred language) or use less memory.&#x20;

Additionally, contributions that enrich the documentation, provide detailed tutorials, or showcase real-world application of _catch22_ are especially appreciated. If you have insights or experience applying _catch22_ to solve real-world problems, sharing these through tutorials or case studies can be incredibly beneficial to both new and existing users.

## Our Code of Conduct 🤝

***

_catch22_ is committed to fostering a welcoming and inclusive community. All contributors and participants are expected to adhere to our Code of Conduct. Before engaging with the _catch22_ project, please take some time to read our[ _Code of Conduct_](contributor-code-of-conduct.md) and ensure that you abide by its terms. We believe in a respectful and harassment-free experience for everyone, and our Code of Conduct outlines our expectations for participant behaviour.&#x20;

## Types of contributions we seek 👨‍👨‍👦‍👦

***

_catch22_ thrives on a wide range of contributions, and we welcome your ideas, feedback and code. Here are some of the ways you can contribute to the project:

* **Bug fixes**: Identifying and fixing bug are critical to maintaining the quality and reliability of _catch22_. We use GitHub issues to track all bugs and feature requests. Feel free to open an issue if you have found a bug or wish to see a feature implemented.  &#x20;
* **Documentation**: Enhancing and updating our documentation to make it more comprehensive, up-to-date and user-friendly. If you find a typo in the documentation, or have suggestions for improvements, do not hesitate to send an email to the mailing list or preferably submit a GitHub pull request.
* **Examples and tutorials**: Creating practical and informative examples or tutorials to assist users in understanding and utilising _catch22_.&#x20;
* **Troubleshooting:** Another way to contribute is to report issues you are facing and give a "thumbs up" on issues that others reported and that are relevant to you. It also helps us if you spread the word by referencing the project from your blogs and articles, link to it from your website, or simply 'star' the project on GitHub to say "_I use it_".
* **Existing feature enhancement**: We welcome improvements aimed at increasing the efficiency of existing time series feature calculations, such as optimising code to run faster (on your preferred language) or use less memory.&#x20;

***

## Bug Reporting Guidelines 📋

***

We value bug reports as they play a crucial role in improving the quality of _catch22_. We use GitHub issues to track bug requests, so if you encounter issues using this package, do not hesitate to submit a ticket to the bug tracker for the language-specific implementation of _catch22_:

* _catch22_ (MATLAB and C)[ Bug Tracker](https://github.com/DynamicsAndNeuralSystems/catch22/issues)
* _pycatch22_ (Python)[ Bug Tracker](https://github.com/DynamicsAndNeuralSystems/pycatch22/issues)
* _Catch22.jl_ (Julia) [Bug Tracker](https://github.com/brendanjohnharris/Catch22.jl/issues)
* _Rcatch22_ (R) [Bug Tracker](https://github.com/hendersontrent/Rcatch22/issues)

Before submitting a ticket, it is recommended to check that your issue complies with the following rules before submitting:

* Verify that your issue is not currently being addressed by an existing ticket.
* Ensure that your bug report complies with the _catch22_ bug report guidelines.

### Submitting an ideal bug report 🌟

When submitting a bug report to the issue tracker in GitHub, please ensure that you follow these guidelines. This will streamline the resolution process and allow community members to provide good feedback. A good bug report shouldn’t leave others needing to chase you up for more information. Please try to be as detailed as possible in your report:

* The ideal bug report should contain a **short reproducible code snippet**, this way anyone can try to reproduce the bug easily. If your snippet is longer than 50 lines, please link to a gist or GitHub repo.&#x20;
* If not feasible to include a reproducible snippet, please be specific about what classes/functions involved.
* If an exception is raised, please **provide the full traceback**.&#x20;
* Include your **operating system, architecture** and **version number**, as well as your (Python/MATLAB/Julia/R) **version**.&#x20;
* Ensure all code snippets and error messages are formatted in [appropriate code blocks](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks).&#x20;

***

## Pull Request Guidelines ✔️

***

To ensure a smooth and efficient review process, we ask that you adhere to the following guidelines when submitting a pull request. While these guidelines specifically pertain to Python (_pycatch22_), they also generalise to implementations of _catch22_ in other languages:

* **Include tests:** If your changes include modifications to existing features, please include appropriate tests. Well-written tests are fundamental to ensuring the reliability and stability of _catch22_. To get an idea of the structure and conventions for writing unit tests, see the [source code ](https://github.com/DynamicsAndNeuralSystems/pycatch22/blob/main/tests/unit_tests.py)for existing _catch22_ unit tests. Note we use [_**pytest**_ ](https://docs.pytest.org/en/7.4.x/)for all unit testing as standard.&#x20;
* **Update documentation**: If your changes involve user-facing features or significant modifications to existing functionalities, please update the relevant documentation sections.
* **Follow Code Standards:** Ensure your code adheres to the coding standards and style guidelines of _catch22_. Consistent coding styles contribute to the readability and maintainability of the project.
* **Compatibility checks**: Verify that your changes are compatible with the supported versions of Python and any other dependencies. All tests should pass on the continuous integration (CI) system after your pull request is submitted.

Submitted pull requests are reviewed on a regular basis. The _catch22_ core team evaluates each contribution for its quality, relevance, and alignment with the project's goals. Feedback will be provided, and we expect contributors to engage in a collaborative review process. If a pull request remains inactive for an extended period after feedback has been provided, it may be closed to keep the project's backlog manageable.&#x20;

***

***
