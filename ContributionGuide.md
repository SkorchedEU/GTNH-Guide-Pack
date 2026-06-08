# Contribution Guide for GuideNH
This file intends to outline the basic requirements and standards for contributing to the content displayed in game via GuideNH. Since the mod is so new, this guide is heavily subject to frequent change as we improve best practices and is designed **ONLY** to govern officially shipped content.

# Main Guidelines
1. Guides are designed primarily to provide useful information to the user, aiming to reduce the amount of external resource checking required by the players.
2. Everybody is welcome to contribute to the guide content, not just core maintainers. On the flipside, your work will be checked and has potential to be rejected if deemed unfit, so for larger changes please consult in the #guide-nh-dev channel on discord first. We'd hate for you to work for nothing!
3. Ideally, you should only change guides on topics you have strong experience in. This obviously does not include for brand new additions, where ideally the author of the changes should be consulted. Exceptions can also be made where the content is just "ported" from some other reputable source, such as the wiki.

# Further Guidelines
Some of these should go without saying, but we said them anyway!
1. Guides should primarily be used to inform players, not dictate their style of play. Avoid statements like "X is better than Y", regardless of how true they are
2. "Puzzles" (such as waterline, or complex automations) should not have direct solving guides in the bundled content. The community is free to use this guide to contribute to a community project for such content, but it will not be shipped by default
3. Guides should be categorised sensibly, either by the mod they fit in (GT5 for EBF for example), or some other coherent theme (such as a changelog). Bear in mind that pages can be assigned multiple categories.
4. This should be blindingly obvious, but do not include swearing, offensive, or generally any political content in guides. This is a safe space for players to avoid the outside world, don't bring it back in!
5. All image names should be written in snake case: `this_is_an_example_name.png`
6. Any external URL links should be to content produced by GTNH as an organisation, unless given explicit authorisation from a developer to do so.
7. If you have any further questions on contribution, please ask in #guide-nh-dev in the discord.

# Working in Guide Language

# Git Basics

- This is not a Git tutorial. Familiarize yourself a bit with Git before trying to do quest updates.
https://git-scm.com/docs/gittutorial
https://try.github.io/

- As an alternate to using Git from the command line, you can use GitHub Desktop as a GUI. It makes things very simple, and you don't need to remember any commands. We will focus on that option in the following.


# Git Definitions

- Repo - The database of all the files and their history. The remote repo is on Github. The local repo is on your drive. They do *not* automatically track each other.
- Source/upstream repo - The original/parent/upstream repo, in this case GTNewHorizons/GTNH-Guide-Pack
- Forked repo - Your personal repo, for example chochem/GTNH-Guide-Pack
- Local repo - The copy of a repo on your local PC
- origin repo - The copy of a repo out on github
- Commit - A bundle of changes that are added to the repo. Each is assigned a unique SHA tag. Initially commits are stored in your local repo.
- Branch - A branch represents an independent line of development. Master is typically the "primary" branch. Any new branch is started off a particular state of an old one. This starting point can also come from a branch in the upstream repo (e.g. the up to date master in the upstream repo!). From there the branch is then independent.
- Fetch - An action to update your local repo with any changes files. This does NOT affect your branch, but will show you any changes such as new commits etc. You can configure it to prune (remove) any "stale" branches.
- Push - An action to send your commits up to the remote repo. This does not need to be done every commit.
- Pull - An action to retrieve new commits from the remote repo and put them into your local repo, updating your version of the branch to match the remote repo.
- Rebase - This takes the changes done in your local branch, and redoes them on top of the selected commit. Do this to include new changes from the original branch in your modified branch.  IE, you are adding guides to the pack, and someone pushes a config change (unrelated to your guides) to master. Checkout your branch. Right-click on the commit for the master branch, select Rebase, and now your guide changes will be on top of the new config change. Ideally there should be no conflicts. If there is a conflict, flag staff.
- Merge - This has to be done when two people are editing the same file. Ideally this should be avoided.
- Pull Request (PR) - Once you have completed your changes, publish your branch to your forked repo. On github, you can goto your forked repo page and do a Pull Request to send the branch to the source repo owners and ask permission for them to add the changes to a branch of their repo, e.g. upstream/master.

# Working on Guides for GTNH with GitHub Desktop
GuideNH works using a mixture of common languages as outlined below:
- MDX - A markdown format that allows writing of JSX in markdown documents. For the majority of content, this can be seen as just markdown with some special tags with some in game uses (see below for more)
- YAML (frontmatter) - A markup language used to add metadata at the top of each markdown file to designate navigation data, title and more.
- SNBT - A minecraft specific data format primarily used as a representation of NBT data to be imported into the guide. Primarily used to add 3D structures into the guide pages
- JSON - A readable (ish) text format for storing data in object form. Used primarily for storing the keyframes of "Ponder" animations.

## Getting Started
The general file layout of a guide with a single page is as follows:

```text
wiki/resourcepack/
`-- assets/
    `-- <modid>/
        `-- guidenh/
            |-- assets/
            |   `-- example_structure.snbt
            `-- _en_us/
                `-- index.md
```
When integrating into the pack directly through TXLoader, it should be as:
```text
config/txloader/load/
`-- <modid>/
    `-- guidenh/
        |-- assets/
        |   `-- example_structure.snbt
        `-- _en_us/
            `-- index.md
```
so that TXLoader can correctly retrieve the content
## Frontmatter Headers
The frontmatter navigation data must be the **first** thing in each markdown file. The smallest useful frontmatter for navigation is:
```yaml
navigation:
  title: Root
```

There are many more things you can put in your header, including linking item ids for the "Hold [G]" mechanic, categorisation, adding to recommended page, and more. A "full" example is shown below to give an idea:

```yaml
item_ids:
  - gregtech:gt.blockmachines:15529
  - gregtech:gt.blockmachines:15530
  - gregtech:gt.blockmachines:15531
  - gregtech:gt.blockmachines:15532
navigation:
  title: Large Boiler
  parent: reworks.md
  icon: gregtech:gt.blockmachines:15529
categories:
    - Structure Reworks
    - New Multiblocks
author: Skorched
date: 2026-05-27
```

## Adding Assets
Place page-local assets next to the page file:

```text
wiki/resourcepack/assets/guidenh/guidenh/_en_us/test1.png
```

Reference them relatively from markdown:

````md
![Example](test1.png)
````

Place shared guide assets under the guide's own `assets/` folder:

```text
wiki/resourcepack/assets/guidenh/guidenh/assets/example_structure.snbt
```

Reference them with a rooted guide path:

````md
<ImportStructure src="/assets/example_structure.snbt" />
````

## Learning the Tags
In order to make GuideNH as powerful as it is, there are lots of custom tags that can be invoked within the markdown file to give some powerful effects. The full list of tags and how to use them can be found in the wiki, but [this page](https://github.com/GTNewHorizons/GuideNH/blob/master/wiki/Tags-Reference.md) is a really helpful reference to start with!

# Contributing to Guide Content
1. Fork the GTNH-Guide-Pack repo to your github account. This is not actually needed if Dream has added you as a developer.
2. Add the repo to your GitHub Desktop by clicking `Code` and then `Open with GitHub Desktop`. When asked, choose `contribute to parent repo`. That way new branches are automatically based on the GTNewHorizons/GTNH-Guide-Pack master branch.
3. Before you begin making changes, you want to click `Fetch origin` to get the latest updates and then start a new branch (`Branch`, then `New Branch`). If you followed 2. that will automatically be based on the GTNewHorizons/GT-New-Horizons-Modpack master branch.
4. Start GTNH. You need the full game (the latest release or a nightly version), not a development environment. You should also check that you are using the latest GuideNH version from https://github.com/GTNewHorizons/GuideNH/releases (you should only use normal versions, not pre-versions).
5. Ideally do one guide or change per commit. Doing regular commits makes it easy to review your changes or to revert specific things! It also backs up your work.
13. Once finished, compare the resulting changes and make sure everything is as expected. by copying this changed repository locally into your game resource packs and loading the guide.
14. Then publish your branch and make a PR to the GTNewHorizons/GTNH-Guide-Pack master branch. For example you can go to your forked repo on github and make sure your branch has shown up there. An option should show up there to "create pull request".
15. List all your changes in the description of the PR. And add screenshots where useful.
16. Write 'fixes #issue' in the PR description to automatically link a ticket to the PR.
17. If changes to your PR are required, you can just make more commits on your branch and push them. Make sure to comment on the changes in the description.
18. After the PR is merged, you can delete your branch.

# New Guides
1. Create a new `<fileName>.md` file within the directory you want this guide to be a part of. This will be the main file you edit, and what is displayed to the player. Ensure that your file is within the correct lang directory for what you intend to write (e.g. inside `_en_us` for American English). If you are creating a new category, make sure you have discussed it with the team in discord, and see above for configuration tips.
2. Any assets you need (structure `.snbt` files, images, or `.json` files) should be stored in an appropriate `assets` folder.
3. Ensure that the frontmatter portion of your guide is the first section, and that it has a meaningful title, correct navigation data, and any item ids you want linked assigned correctly.
4. When happy to test your changes, copy the working directory from its root (`GTNH-Guide-Pack` and everything inside) into your games `resourcepacks` folder, and refresh your resource pack in game with `F3 + T`. The changes should then be visible in game.
5. Once happy with your changes, add, commit, and push your changes as outlined above, and create a PR that describes what you've added in a meaningful way.
