# Daisy Git Workflow

## Summary

This page describes the workflow used in conjunction with `Git
<http://git-scm.com>`_ and `GitLab <https://www.gitlab.com/>`_ for
those who have push access to the repository.

Go `here
<https://education.github.com/git-cheat-sheet-education.pdf>`__
or `here
<https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet>`__
for a cheatsheet of Git commands.

## Description

We follow the [GitHub flow](https://guides.github.com/introduction/flow/index.html), using branches for new work and pull requests for verifying the work.

The steps for a new piece of work can be summarised as follows:
1. Push up or [create](https://docs.gitlab.com/ee/user/project/issues/) an [issue](https://code.ihep.ac.cn/hepscc/Daisy/-/issues) here
2. Create a branch from main using the naming convention described at Naming Branches
3. Do the work and commit changes to the branch. On commit, the pre-commit framework will run, it will check all your changes for formatting, linting, and perform static analysis. Push the branch regularly to IHEP GitLab to make sure no work is accidentally lost.
4. When you are finished with the work, ensure that all of the unit tests, documentation tests and system tests if necessary pass on your own machine
5. Open a merge request (Merge Requests) from the [GitLab branches](https://code.ihep.ac.cn/hepscc/Daisy/-/branches) page
    - This will check with the buildservers for cross-platform compatibility
    - If any issues come up, continue working on your branch and push to GitHub - the pull request will update automatically
    
    
## Naming Branches
When naming [public branches](https://code.ihep.ac.cn/hepscc/Daisy/-/branches) that will be pushed to IHEP GitLab, please follow the convention of ``issuenumber_short_description``. This will allow others to discover what the branch is for (issue number) and quickly know what is being done there (short description).

## Merge Requests

For an general overview of using Merge requests on GitLab look [here](https://docs.gitlab.com/ee/user/project/merge_requests/).

When creating a merge request you should:
- Ensure that the title succinctly describes the changes so it is easy to read on the overview page
- The title should not contain the issue number
- Reference the issue which the pull request is closing, using one of these [keywords](https://docs.gitlab.com/ee/user/project/merge_requests/creating_merge_requests.html#from-an-issue)
- State the user and facility (if relevant) who initiated the original issue, if they are named in the issue. Please do not put full email addresses on the Pull Request, as it is publicly accessible. If the user would not be easily identified by someone picking up the ticket, be prepared to act as a point of contact with the reporter.
- Ensure the description follows the format described by the PR template on GitLab

For further information about the review process see reviewing a pull request.
