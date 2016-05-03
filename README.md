# Git Standards & Procedures


### Configuration

```sh
git config --global user.name "My Name"
git config --global user.email "myemail@host.com"
git config --global push.default simple
```


### Fork upstream repository

[https://bitbucket.org/ketchumdigital]()


### Grant Ketchum Digital read access to your fork

In BitBucket > Settings > Access management, add:

```
ketchumdigital
```


### Clone forked repository

```sh
git clone <forked_repo>
```


### Set up remotes

```sh
git remote add upstream <upstream_repo>
git remote add pantheon <pantheon_repo>
```


### Workflow


#### Sync your fork

```sh
git fork-sync
```

or manually:

```sh
git checkout master
git fetch --all
git rebase -p upstream/master
git push
```


#### Work on topic branches

```sh
git checkout master
git checkout -b <topic_id-topic_branch>
```


#### Make atomic commits

```sh
git add paths_to_add
git commit -m "commit message"
```


#### Sync your topics

```sh
git fork-sync -t <topic_branch>
```

or manually:

```sh
git checkout master
git fetch --all
git rebase -p upstream/master
git push
git rebase -p master <topic_branch>
```


#### Push

The first time you push your topic branch to your origin:

```sh
git checkout <topic_branch>
git push -u origin <topic_branch>
```

Subsequently after rebasing:

```sh
git checkout <topic_branch>
git push -f
```


#### Test and review your code

If you need to test your code in the wild before creating a PR create a multidev instance in Pantheon (with the same name as your topic branch) and push your topic branch.

```sh
git push pantheon <topic_branch>
```


#### Make a pull request

Only when your topic branch is ready for code review and merging.


#### Start clean

```sh
git nuke <topic_branch>
```

or manually:

```sh
git branch -d <topic_branch>
git push origin :<topic_branch>
```

Do this for any topic branch you wish to delete locally and remotely.


### For Maintainers / Integrators

- You must have at least read access to a developer's fork.
- Pull requests should include the tip of master before the developer’s changes.
- Pull requests from master to master should be rejected.
- If you need to manually merge, always do ‘git merge --no-ff <topic_branch>’ to produce an explicit merge commit object.


### Workflow


#### Clone upstream repository

```sh
git clone <upstream_repo>
```


#### Setup remotes

```sh
git remote add origin <upstream_repo>
git remote add pantheon <pantheon_repo>
```


#### Sync upstream

```sh
git checkout master
git fetch --all
git reset --hard origin/master
git push pantheon master
```


#### Mirrors

If you need to mirror a repository, create a blank repo at the target and mirror push to it:

```sh
git fork-sync -u <mirror_name> -u <mirror_name> ...
```

or manually:

```sh
cd local_repogit remote add <mirror_name> <user>@<host>:<path_to_blank_mirror>
git push --mirror <mirror_name>
```
