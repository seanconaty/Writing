# Squashing Commits

Sure we look at a PR's total diff, but sometimes it's nice to look at commits. They're a way to organize your changes. As you know, your changes change as you develop. These changes, however, can be abstracted away so that everyone thinks you write immaculate code. Here's how I do it:

## My Good Work

    # I'm beginning work on a new poem
    echo -e "Roses are green\nViolets are blue" > static/love_poem.txt
    git commit -am "writing a new love poem about git"

    # Hmmm, what should the next two lines be?
    echo -e "Too many commits\nAre hard to review\n" >> static/love_poem.txt
    git commit -am "finishing love poem"

    # Uh oh. I've spotted a problem. Need to change green to red
    sed -i 's/green/red/' static/love_poem.txt
    git commit -am "ooops! silly me roses are RED not GREEN, whoopsie daisy"

    # Just great! Now everyone is going to see my bonehead mistake!
    git log -n3  # See the last 3 commits

    # Unless I interactive rebase!
    git rebase -i upstream/master

## Interactive Rebase Screen

This last line will open a file in your editor with a list of commits and some handy instructions on how to mix and match your commits.

    pick 32e7403 writing a new love poem about git
    pick a2a1ce9 finishing love poem
    pick 1caac10 ooops! silly me roses are RED not GREEN, whoopsie daisy
    
    # Rebase 24a1d10..1caac10 onto 24a1d10
    #
    # Commands:
    #  p, pick = use commit
    #  r, reword = use commit, but edit the commit message
    #  e, edit = use commit, but stop for amending
    #  s, squash = use commit, but meld into previous commit
    #  f, fixup = like "squash", but discard this commit's log message
    #
    # If you remove a line here THAT COMMIT WILL BE LOST.
    # However, if you remove everything, the rebase will be aborted.
    #

So I want to `squash` my third commit into my first. That way no one knows I got the rose color wrong. All I do is move the 3rd commit below the first commit and write `squash` on it, instead of `pick`.

    pick 32e7403 writing a new love poem about git
    squash 1caac10 ooops! silly me roses are RED not GREEN, whoopsie daisy
    pick a2a1ce9 finishing love poem

## Squash Commit Message Screen

I write this and quit then I see the following. Git wants me to rewrite the commit message since it now contains what was previously 2 commits.

    # This is a combination of 2 commits.
    # The first commit's message is:
    
    writing a new love poem about git
    
    # This is the 2nd commit message:
    
    ooops! silly me roses are RED not GREEN, whoopsie daisy
    
    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    # Not currently on any branch.
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       new file:   website/hearsaylabs.com/fanmgmt/static/love_poem.txt
    #

Really I don't care about the second commit message so I'm going to delete that and stick with the original.

    writing a new love poem about git
    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    # Not currently on any branch.
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       new file:   website/hearsaylabs.com/fanmgmt/static/love_poem.txt
    #

## Now, Let's Take Another Look

Now if I look at my commit history, it looks like I did everything right from the get-go!

    git log -p -n2
    commit 4b52f96117b0a5450d9a9b744b14bcf2d165ed00
    Author: Sean Conaty <seanconaty@gmail.com>
    Date:   Tue Apr 24 20:04:37 2012
    
        finishing love poem
    
    diff --git a/site/static/love_poem.txt
    index b0ad8be..deb84ba 100644
    --- a/site/static/love_poem.txt
    +++ b/site/static/love_poem.txt
    @@ -1,2 +1,5 @@
     Roses are red
     Violets are blue
    +Too many commits
    +Are hard to review
    +
    
    commit 159779c146af370714904bcd07322de27f2457c9
    Author: Sean Conaty <seanconaty@gmail.com>
    Date:   Tue Apr 24 20:03:34 2012
    
        writing a new love poem about git
    
    diff --git a/site/static/love_poem.txt
    new file mode 100644
    index 0000000..b0ad8be
    --- /dev/null
    +++ b/site/static/love_poem.txt
    @@ -0,0 +1,2 @@
    +Roses are red
    +Violets are blue

## The End

    Roses are red
    Violets are blue
    Too many commits
    Are hard to review
