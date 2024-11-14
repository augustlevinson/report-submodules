report-submodules
===

Choosing how to structure your version management can have a huge impact on the workflow of your team down the line. This report discusses differences, pros and cons of organizing a project as a monorepo vs. using multiple GitHub repositories as submodules in a parent repository.

Contents
---
1. [Prerequisites](#prerequisites)
2. [Submodules setup](#submodules-setup)
3. [Monorepo setup](#monorepo-setup)

Prerequisites
---
A terminal that supports [git](https://git-scm.com/) and a [GitHub](https://github.com/) account.

Submodules setup
---
Git submodules are repositories within repositories or, more accurately, pointers to other repositories within a repository. They allow you to include other repositories in your project, which can be useful for managing dependencies or splitting a project into smaller parts.

### 1. Create a parent repository

Create a repository to import the other repositories from. If you are unsure of how, follow [this guide](https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories).

Clone the repository as described in [this guide](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository).
___
### 2. Create separate repositories for each microservice

Create a repository each for every microservice, for example *backend-api*, *admin-ui* and *customer-ui*.

___
### 3. Add submodules to the parent repository

The URL to add can be found in the Local > HTTPS tab of the Code section of your submodule-to-be:

![The URL of the child repository](img/add-submodule.png)

Navigate to your parent repository:

```
cd path/to/parent-repo
```

Add the other repositories as submodules using their URLs:
```
git submodule add https://github.com/user/backend-api.git
git submodule add https://github.com/user/admin-ui.git
git submodule add https://github.com/user/customer-ui.git
```
___
### 4. Initialize and update submodules.

After cloning the parent repository, initialize and update all submodules:

```
git submodule update --init --recursive
```

Push the changes to the parent repository:

```
git add .
git commit -m "Added microservice repos as submodules"
git push
```

Navigating to the parent repository, you can see the submodules listed as folders:

![The submodules in the parent repository](img/submodules-repo.png)
*The submodules are snapshots of the specific branch at the time of adding them. To update them to the latest version, you first need to have their repos updated and then push those changes to the parent repo.*

To update the submodules to their latest changes on main, navigate to each repository and run:

```
# Navigate to the submodule repo
cd path/to/repo

# Fetch the latest changes from the remote
git fetch origin main

# Update the submodule to the latest commit
git checkout main
git pull origin main
```

After updating the submodules, navigate back to the parent repository and commit the changes:

```
# Navigate back to the parent repository
cd path/to/parent-repo

# Add the updated submodule, commit and push
git add path/to/submodule-repo
git commit -m "Update avec-rest-api submodule to latest commit"
git push
```

![The updated submodules in the parent repository](img/updated-submodule.png)
*Now you should see the updated submodule in your parent repository.*

This is just a basic guide to get going. For a more extensive guide on the git submodule tool, consult the [official git documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules).

Monorepo setup
---
A monorepo is a single repository that contains all the code for a project. To separate the code into different parts, you can use directories or packages.