# Mattermost Documentation

This repository generates the documentation available at https://docs.mattermost.com/. All documentation is available under the terms of a [Creative Commons License](https://creativecommons.org/licenses/by-nc-sa/3.0/).

If you have any questions, create an account on the [Mattermost Community server](https://community.mattermost.com/signup_user_complete/?id=f1924a8db44ff3bb41c96424cdc20676), then join the writing team in the [Documentation Working Group](https://community.mattermost.com/core/channels/dwg-documentation-working-group) channel. We look forward to working with you!

# Table of Contents

 * [Contributing](#contribute-to-mattermost-product-documentation)
     * [Get started](#get-started)
     * [Edit content directly on GitHub](#edit-content-directly-on-github)
     * [Create documentation PRs](#create-documentation-pull-requests)
     * [Use GitHub PR Labels](#use-github-pr-labels)
     * [Comment on Pull Requests](#comment-on-pull-requests)
     * [Review Pull Requests](#review-pull-requests)
 * [Build locally](#build-locally)

## Contribute to Mattermost product documentation

### Get started

You can edit or create Mattermost documentation directly in GitHub, or by downloading the `mattermost/docs` repository onto your machine and using an IDE such as VSCode.

If this is your first time contributing to Mattermost, first read and sign the [Mattermost Contributor Agreement](https://mattermost.com/mattermost-contributor-agreement/), so you can be added to the Mattermost [Approved Contributor List](https://docs.google.com/spreadsheets/d/1NTCeG-iL_VS9bFqtmHSfwETo5f-8MQ7oMDE5IUYJi_Y/pubhtml?gid=0&single=true).

### Edit content directly on GitHub

The quickest way to begin is editing directly on GitHub on your fork of the Mattermost docs repo. Select the **Edit** icon on the top right corner of the page you want to edit in the Mattermost documentation.

If this is the first time you're contributing, follow these steps: 
1. Select **Fork** in the top-right corner of the GitHub repository page to fork the current repository.
2. Navigate to file you want to edit, then select the Pencil icon (**Edit the file**) to open the editing interface.

### Create Documentation pull requests

1. When you're ready to submit your changes, add a descriptive title and comments to summarize the changes made.
2. Select **Create a new branch for this commit and start a pull request**.
3. Check the **Propose file change** button.
4. Scroll down to compare changes with the original document.
5. Select **Create pull request**. 

### Comment on pull requests

Once a pull request is submitted, multiple committers may comment on it and provide edits or suggestions which you can commit directly. You can also add line comments. Take a look at [Commenting on pull requests](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/commenting-on-a-pull-request) for more details.

### Review pull requests

Once a pull request has been submitted and the correct label assigned, the review process begins. This includes aligning the content with the Style Guide, validating processes, and tagging any other relevant committers. Read more about the review process and expectations [in the Mattermost Developer documentation](https://developers.mattermost.com/contribute/getting-started/code-review/). 

Once the review process is complete, and depending on the type of issue it is (e.g., a typo fix vs. a new feature), the change is either merged into master and pushed immediately or merged into the release branch and pushed in alignment with a future release. The branch is then deleted. 

## Build locally

If you've downloaded the `mattermost/docs` repository to edit Mattermost documentation on your local machine, you can generate the HTML files from the `source` directory. You can review your changes as a live or static preview before committing them or creating new pull requests.

> [!NOTE]
> You can generate the docs on Linux, Mac, and Windows (using PowerShell); however, builds on Windows are considerably slower because only a single processing core is used.
> 
> For faster local docs builds on Windows, we strongly recommend [installing WSL](https://learn.microsoft.com/en-us/windows/wsl/install) to create an Ubuntu virtual machine (VM), where you'll configure the following prerequisites. An Ubuntu VM will use all available processing cores, resulting in faster local builds.

### Build prerequisites

The following software is required to build the documentation:

- Git [[download]](https://git-scm.com/downloads)
- Python 3.11 or later [[download]](https://www.python.org/downloads)
- Pipenv [[download]](https://pipenv.pypa.io)
- GNU Make 3.82 or later

### Build instructions

1. Open a native or VM terminal window, then clone a forked copy of the documentation repository:
    ```shell
    git clone https://github.com/mattermost/docs.git
    ```

2. In the terminal window, navigate into the cloned repository:
    ```shell
    cd docs
    ```

3. Install [pipenv](https://docs.pipenv.org/) by using one of the following commands based on your operating system:

    For Mac users and Ubuntu VM users where Homebrew is installed:
    ```shell
    brew install pipenv
    ```

    For other operating systems:
    ```shell
    pip install --user pipenv
    ```

4. Install required Python packages. You have two options:

   Use `pipenv sync --dev` when:
    - You need exact reproducibility of the production build environment.
    - You're setting up a CI/CD pipeline.
    - You want to avoid accidentally updating dependency versions.
    - You're working in a team where everyone should use identical package versions.

   Use `pipenv install --dev` when:
    - You're setting up a local development environment for the first time.
    - You want to allow minor package updates within the version constraints.
    - You're working independently from production and want the most recent compatible versions.

5. Run ``git submodule update --init --recursive`` to ensure that the Git submodules for Mattermost Agents are initialized and updated.

6. Build the documentation set. You have three build commands available at the terminal:

    - Use `gmake html` to generate HTML files in the `/build` directory. Only file you've modified are re-built.
    - Use `gmake clean html` to delete all static HTML output in the `/build` directory and re-build all files. This command is particularly useful when you're making changes to the left navigation pane and want to ensure you're not reviewing cached results.
    - Use `gmake livehtml` to review a live preview published to `http://127.0.0.1:8000` that automatically updates as new changes are saved in your local IDE. Always run `gmake clean html` first before reviewing a live preview.

> [!NOTE]
> Windows users who aren't building the docs in an Ubuntu VM also require `make` installed for the build commands above to work correctly. To install `make` via Chocolatey:
>
> 1. Install [chocolatey](https://chocolatey.org/).
> 2. In a Windows terminal, select the downward chevron, and hold `CTRL` while selecting **PowerShell** to run commands as an admin.
> 3. Accept the admin prompt.
> 4. Run the following command: `choco install make`
> 5. Exit the terminal.
>
> When building the Mattermost Product Documentation locally, Windows users will see slower build speeds because only a single processing core is used to build the docs. Mac & Linux users will see faster build speeds because multiple cores are used to build. This is a limitation of Sphinx on Windows platforms.

7. When working with static build results, navigate to the `build` directory:
    ```sh
    cd build
    ```
   
8. Then, preview your changes by opening the `build/html/index.html` file.

Build errors are written to the `build/warnings.log` file. Errors regarding redirects are written to the `build/redirect-warnings.log` file.
