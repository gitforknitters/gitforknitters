# git4knitters
We describe git in terms of knitting, and we do not limit our analogies. We love all fibreholics! Join us!

Initial [twitter convo](https://twitter.com/yvonnezlam/status/589585522735063040).

## Rationale

Learning can be difficult. Learning Git even more-so. This project attempts to couch Git in knitting terms. Although it's a bit of fun, the use of analogies and foundation learning is a known and respected tool for curriculum designers and educators.

## Terms

A glossary of git terms in the context of knitting.

Git also offers its own [glossary of git terms](https://www.kernel.org/pub/software/scm/git/docs/gitglossary.html).

### stash

*In knitting terms:* Knitters will often refer to "having a stash" of yarn. When starting a new project we might "shop the stash" instead of going to the store. Those of us who have *a lot* of supplies may say that we have "achieved SABLE" (Stash Amassed Beyond Life Expectancy).

*In Git terms:* you are working on a branch, coding a specific feature. All of a sudden you realise that you want (or need) to work on something else for a bit. You aren't at a logical stopping point so you don't want to `commit` your work. Instead, you can use `stash` to temporarily shelve your work.

`$ git stash --include-untracked`

The changes you were working on are now included in your `stash`. You may apply them to the branch you are currently on, and they are available to *any* branch in your repository. So if you realise you had been working in the wrong branch, this is also a great way to move your changes to the correct branch.

Once you've stashed your uncommitted changes, the working directory is clean and you may change branches to work on a different set of tasks.

When you are ready to re-apply your changes, you may "shop the stash" to locate the relevant stash. First, take a quick inventory of all the available stashes:

`$ git stash list`

If there is only one result returned, you can safely use this stash. To keep the stash small, use the subcommand `pop` which will apply, and then delete the stored stash.

`$ git stash pop`

If you have more than one stash in your inventory, you may examine any one stash item with the subcommand `show`.

`$ git stash show [stash_id]`

Once you've located the correct stash, you can apply it to your current branch.

`$ git stash pop [stash_id]`

[Git command reference for stash](https://www.kernel.org/pub/software/scm/git/docs/git-stash.html).

### commit
*In knitting terms:* Some knitters like having multiple projects in flight.  Suppose you’re working on both a challenging lace shawl while also making progress on a baby blanket. You’ve been working on the shawl at home, but tonight you’re going to your knitting circle where you enjoy chatting and not having to concentrate so hard, so you’ll bring the baby blanket to work on.  Setting down your shawl, you up your stitch counter and/or mark the row you just finished on the pattern.

*In Git terms:*
Saving your changes is a two-step process in git.  First, you’ll want to use the add command to tell git that you have changes you want to be included in the next commit.

`$ git add <file>`

`$ git add <directory>`

or

`$ git add .` to include all changes in the next commit.

Once the files have been “staged”, you can commit them to your local repository with a message:

`$ git commit -m “<message>”`

which is akin to you marking your stopping point in the pattern and enabling you to set down that lovely shawl for now, knowing that you won’t have lost your place the next time you work on it.

[Git command reference for add](https://www.kernel.org/pub/software/scm/git/docs/git-add.html)

[Git command reference for commit](https://www.kernel.org/pub/software/scm/git/docs/git-commit.html)

### reset

*In knitting terms:* You're knitting lace. You realise your pattern isn't matching up. You're offset by a few stitches. Somewhere you've made a mistake. Bugger. Instead of ripping apart the whole piece, you can simply "tink" back to the mistake where you can fix it up and then continue knitting. "Tink" is a clever reversal of the word "Knit". When you are "tinking" you are merely "knitting backwards".

*In Git terms:* A little while ago you could have done a slightly better job than what's there. You can't use `commit --amend` though because it's not the most recent commit.

Take a look at the log to see where the bad commit is, and where the last good commit is.

`$ git log --oneline`

If the mistake is only a couple of commits ago, you can use the syntax of `HEAD` and `^` for each stitch you need to tink. The following would tink back two commits:

`$ git reset HEAD^^`

The git manual page [gitrevisions](https://www.kernel.org/pub/software/scm/git/docs/gitrevisions.html) explains different ways to count "backwards" into your commit history.

Alternatively, you can locate the last good stitch that you want to keep, and tink back to this commit. If you are using this format, you need to go one _past_ the bad stitch, so that you make sure you tink the bad stitch. If you choose the bad stitch as the commit id, it will still be on the needles after running the command.

`$ git reset [commit_id_good_commit]`

These commits will be removed from history as if they never happened. The files are `reset` in your working directory. You may edit them if you want to. When you're ready to proceed, add the files back onto the stage and commit your work.

`$ git add --all`
`$ git commit`

Your text editor will open. Write a detailed commit message about the changes you made.

Perhaps, obviously, it is not appropriate to tink (reset) work which has already been shared with others as other knitters already have the stitches on their needles and your changing of history will … shall we say … make life "complicated". If you've already shared this branch, it's more appropriate to use `revert`.

[Git command reference for reset](https://www.kernel.org/pub/software/scm/git/docs/git-reset.html).

### revert

*In knitting terms:* Your knitted piece is done. There is no freaking way you're going to rip *anything* back, but there's a little corner which needs to be changed. You can use duplicate knitting to mask previous stitches. You might do this to add colour work, or embroidery, or that kind of thing.

*In Git terms:* Once you've shared a branch, you should not change history. Instead, it is appropriate to apply a patch we "redoes" commits. As this is similar to duplicate stitching, you will choose the *exact* commit that you want to modify.

[Git command reference for revert](https://www.kernel.org/pub/software/scm/git/docs/git-revert.html).

### rebase --interactive

*In knitting terms:* You're knitting cables. You realise that you have incorrectly twisted a cable several rows back. You *could* rip all the way back to that point in your knitting, or you could drop the stitches above the cable, ladder them back to the incorrect cable, put them back on the needles, retwist the cable, and then knit it back up again. There is a good description (with photos!) of this technique at [All is not lost](http://www.yarnharlot.ca/blog/archives/2006/06/20/all_is_not_lost.html).

*In Git terms:* You've been working along and all of a sudden (there's a lot of 'all of the sudden' in Git, isn't there?) you realise one of your past commits is not right for some reason. You can use `rebase` with the parameter `--interactive` to re-work the commit.

Locate the last good commit (i.e. the one BEFORE the commit you want to fix).

`$ git log --oneline`

Start the rebasing process:

`$ git rebase --interactive [good_commit_id]`

A text editor will open up with a list of commits. Next to the commit you want to fix, replace `pick` with `edit`.

Save your change and close your editor. You will be taken back in time to this commit.

Make the changes you need to make, then add the changed files to the repository.

`$ git add --all`

Then proceed with the rebasing process.

`$ git rebase --continue`

[Git command reference for rebase](https://www.kernel.org/pub/software/scm/git/docs/git-rebase.html).

### pull request

*In knitting terms:* Knitting a single item is generally a solo activity.  There are times, though, when we collaborate with others on a larger project.  Suppose you want to contribute to a community-supported yarn bomb for a tree.  You knit up a snazzy striped panel that you envision will be used to cover a branch.  You bind off, give your piece to the coordinators, and ask that they seam your piece to other pieces of the yarn bomb.

*In Git terms:*
When you commit changes, these are saved in your local repository.  In order to collaborate with others or publish your changes to the public, you’ll need to do a push.

`$ git push`

Generally, for public projects, you first fork and clone the repo.  This allows you to work independently until you're ready to collaborate.  When you push your changes, they will get pushed to your remote fork (the one on the server).  From there, you can go to github.com and open a pull request (PR), which is akin to telling your yarn bomb colleagues that you have a piece to contribute and that you'd like for them to join it to other pieces.

[Git command reference for push](https://www.kernel.org/pub/software/scm/git/docs/git-push.html)

[Github help for pull requests](https://help.github.com/articles/about-pull-requests/)

### merge conflict

*In knitting terms:* The coordinators of the tree yarn bomb love your work and want to include it.  However, they have already joined another contributor’s piece to the branch you wanted to add yours to.  Because they want visual continuity, and your piece will color clash with the other piece they just added, they ask that you modify the first few rows of striping in your piece.

*In Git terms:* Since there have been changes to the yarn bomb in the area you want your piece joined to, you'll first need to rebase to get those changes (find out what has changed).  In our example, you will encounter a a merge conflict.  You make the requested changes in your piece so the striping is aesthetically pleasing, mark the conflict as resolved, then re-push your changes.  This also updates your pull request.  The contributors are happy and seam your piece to the rest of the yarn bomb.  Yay!

[Git command reference for merge](https://www.kernel.org/pub/software/scm/git/docs/git-merge.html)

[Git tutorial for rebasing vs. merging](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

## Contributing

You are welcome, and encouraged, to contribute your own analogies! The process of contributing to this project will help you learn the Git commands it describes. Please do:

1. Fork the project.
1. Clone the repository.
1. Edit the heck out of the project.
1. Commit your changes.
1. Push your changes back up to your copy of the repository on GitHub.
1. Submit a pull request to the main repository.

This project is all in good fun and licensed for maximum sharing and reusability. It is licensed as [CC-BY](http://creativecommons.org/licenses/by/4.0/). You are encouraged to adopt, and adapt and use it as a starting place to further your own understanding of git.

## Contributors

- Emma Jane Hogbin Westby ([emmajane](https://github.com/emmajane)) - [Git for Teams](http://gitforteams.com)
- Yvonne Lam ([yzl](https://github.com/yzl))
- Elaine Nelson ([epersonae](https://github.com/epersonae]))
