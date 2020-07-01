# How to work together on a coding project

**I have always wanted to try coding together as a team on a project. Something I've never done before. My repositories & commits basically consist of big feature updates in one commit. Sometimes pushing over 1000 new lines in one go.**

With the help of [@kriskuiper](https://github.com/kriskuiper) who already had quite a bit of experience on this matter, I have learned a lot. 

### Overview
- [Create guidelines](#create-guidelines)
- [Branches & Pull requests](#branches--pull-requests)
- [Commit often](#commit-often)

### Create guidelines
So before you start a repository together set up some guidelines. Github has a nice overview of [what to do[(https://help.github.com/en/github/building-a-strong-community/setting-guidelines-for-repository-contributors). (No need to reinvent the wheel).

They are a big help to creating a more systematic approach to your project. Some guidelines to create are:
1. **Naming conventions for commits.** See how [Angular](https://github.com/angular/angular/commits/master) does this. They start with a small word 'refactor:' to tell others what this commit did.
2. **Pull request formats.** With a description, a how to test & maybe an what next section, you can prevent a lot of confusion.
3. **Tags for issues.** By adding tags to issues it will be a lot easilier visible what the importance of issues are. A new feature request would be a lot less important than a bug fix.

### Branches & Pull requests
So how do we work together? Well there's always one single main branch called 'master' _(subject to change with the BLM movement spiking interest on this naming convention)_.

To prevent critical bugs it's paramount that nobody pushes directly on the master branch. It could cause a lot bugs and crashes. You will need to make a [branch](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-branches). It's a clone of the master branch where you can edit to your hearts desire without worrying about breaking the project. 

When you are satisfied with what changes you made you [commit](https://developer.github.com/v3/git/commits/) all the changes to the branch. Commits are milestones you create for your project that you can later on easily revert back to. Like checkpoints in video games. 

Before we shoot in a pull request to the project we need to test what we made. It's a good idea to merge your branch with the master branch if you're trailing far behind and fix some of the possible merge conflicts. I would highly recommend an editor like [Visual Studio Code](https://code.visualstudio.com/) to solve potential merge conflicts, as they made it easilier visible where the changes/conflicts are.

#### Pull request
After we have tested and made sure nothing broke, it's time to send in a pull request. 
You need to [push your branch](https://forum.freecodecamp.org/t/push-a-new-local-branch-to-a-remote-git-repository-and-track-it-too/13222) to the project folder on github. When you add your pull request you should see the guidelines you have previously set up in the text field.

Fill in the details and maybe request a collaborator to review the pull request. Github will automatically check for any merge conflicts. If there are any it will notify you in the pull request.

It's good to have your pull request be reviewed atleast once before you merge, there are even settings that prohibit this if you so see fit.

### Commit often
Yes, do it. I didn't do so before, and in the end your project would look like this:
1. First commit
2. End of project.

Duh, but what if I had to go back to before I added this particular feature. Well that's why committing is important, and naming the commits [right](https://chris.beams.io/posts/git-commit/). 

Visual studio code has a feature that shows you a clear overview of edited files and being able to commit them directly from the editor.

![An example of how the commits work in visual studio code](https://www.datocms-assets.com/30893/1593616987-git-commit.png)

Highly recommend to start using this.

### Good luck
Most of all just try it, learn as you go. Set up some conventions together and stick to them. In the end it's not rocket science (luckily). 

