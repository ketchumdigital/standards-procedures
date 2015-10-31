# Standards & Procedures

## Git

### Configuration

```sh
git config --global user.name "My Name"
git config --global user.email "myemail@host.com"
git config --global push.default simple
```

### Workflow

#### Sync Your Fork

```sh
git fork-sync
```

#### Work on Topic Branches

```sh
git checkout master
git checkout -b topic-name
```

#### Make Atomic Commits

```sh
git add paths_to_add
git commit -m "topic-id: commit message"
```

#### Sync Your Topics

```sh
git fork-sync -t topic_branch
```

The first time you push your topic branch to your origin:

```sh
git checkout topic_branch
git push -u origin topic_branch
```

Subsequently after rebasing:

```sh
git checkout topic_branch
git push -f
```

You must use ‘-f’ as you’ve changed your commit history on your topic branch.

Make a Pull Request

Start clean

```sh
git nuke topic_branch
```

Do this for any topic branch you wish to delete locally and remotely.

### For Maintainers / Integrators

- Pull requests should include the tip of master before the developer’s changes. This show proper syncing of their topic branches
- Pull requests from master to master should be rejected
- You must have at least read access to a developer's fork
- If you need to manually merge,always do ‘git merge --no-ff topic_branch’ to produce an explicit merge commit object
- Don’t forget to keep mirrors in sync!
- Staging and Production branches are never rebased

If you need to mirror a repository, create a blank repo at the target and mirror push to it:

```sh
git fork-sync -u mirror_name -u mirror_name ...
```

### Working with Staging and Production Branches

With continuous integration (CI) and deployment (CD) it is often desirable to have the CI tool trigger builds and deploys off of specific branches. It’s also good practice to have explicit branches for mainline development (usually master, sometimes something else depending on client workflow), staging and production release candidates. For the examples below we presume master is used for mainline development - adjust the name for the mainline development branch accordingly.

Create a staging branch for the first time, sync your fork and:

```sh
git checkout master
git checkout -b staging
git push -u origin staging
git push upstream staging
```

‘-u’ should only be used for your fork. Also push to any mirrors.

When code is ready to be shipped to staging, sync your fork and merge from master with an appropriate commit message and making sure everything is up to date before doing so:

```sh
git checkout staging
git reset --hard upstream/staging
git merge --no-ff master
git push
git push upstream
```

Also push to any mirrors.

Creating aproduction branch for the first time sync your fork and:

```sh
git checkout staging
git checkout -b production
git push -u origin production
git push upstream production
```

‘-u’ should only be used for your fork. Also push to any mirrors.

When code is ready to be shipped to production, sync your fork and merge from staging with an appropriate commit message and making sure everything is up to date before doing so:

```sh
git checkout staging
git reset --hard upstream/staging
git checkout production
git reset --hard upstream/production
git merge --no-ff staging
git push
git push upstream
```

Also push to any mirrors.

### Hotfixing

If hotfixes need to be made on staging or production, those should be done off of the appropriate branch. In this case let’s say there was a production hotfix that needs to be made. Create a hotfix branch off of master after syncing your fork:

```sh
git checkout production
git checkout -b hotfix_name
git push -u origin hotfix_name
git push upstream hotfix_name
```

Also push to any mirrors.

Developers working on the hotfix will work on their fork on the hotfix branch. Pull requests, however, should be from their fork to the upstream hotfix branch. Once the hotfix is ready to be applied to production after syncing your fork:

```sh
git checkout production
git reset --hard upstream/production
git merge --no-ff hotfix
git push
git push upstream
```

Also push to any mirrors then backport the hotfix to master and clean up:

```sh
git checkout master
git merge --no-ff production
git push
git push upstream
```

Also push to any mirrors and delete the hotfix branch at any mirrors:

```sh
git branch -d hotfix
git push origin :hotfix
git push remote :hotfix
```

Substitute staging as needed.