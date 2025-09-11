This is a way of keeping a copy of all my references in a bib file in GitHub.
Then I can use that in Overleaf (this is similar to the features they offer for Zotero or Mendeley bibliography synching, but does not require having to have a paid subscription to overleaf :). 

## Setup from scratch:

1.  Add the following lines to your zsh config file (located by default at `~/.zshrc`).  First set variables for the location of the work tree and the git dir.
  (note, I chose to put the work tree in the bibtex/bib folder in my TEXMFHOME dir. You can check where this is with `kpsewhich -var-value TEXMFHOME`. If it does not already exist consider setting it up the standard way, using [amunn/make-local-texmf](https://github.com/amunn/make-local-texmf)):
    ```zsh
    # Set locations of allbib work tree and git dir
    mytexmf=`/Library/TeX/texbin/kpsewhich -var-value TEXMFHOME`
    allbib_worktree=$mytexmf/bibtex/bib/
    allbib_git_dir=$HOME/allbib.git
  
    # for my allbib.git repo, which just tracks the dir where my all.bib file is
    alias allbib-git='git --git-dir=$allbib_git_dir --work-tree=$allbib_worktree'
    alias allbib-refresh='allbib-git add $allbib_worktree && allbib-git commit -m "routine update" && allbib-git push'
    ```

2.  Then close your terminal and open a new terminal (or reload the zsh config file), so those variables are available.

3.  Then, initialize a bare git repository to track your bibfiles, located in the place you chose above.
    ```bash
    git init --bare $allbib_git_dir
    ```

4.  Add a bib file to be tracked.  Say you have a file `all.bib` in the allbib-work-tree-dir you specified above.  Add it and commit like this:
    ```bash
      allbib-git add all.bib
      allbib-git commit -m 'added all.bib'
    ```

5.  Make Git not show all the untracked files, then add the remote (make the new repo called allbib.git on github first)
    ```bash
    allbib-git config status.showUntrackedFiles no
    allbib-git remote add origin https://github.com/$GITHUB_USERNAME/allbib.git
    ```

## Setting up on new computer (pulling already existing version)

-   Run step 1 above to set locations and make aliases.

-   Then, clone this repo as a bare repo in your desired location locally

    ```bash
    git clone --bare git@github.com:postylem/allbib.git $allbib_git_dir
    ```

-   Get the working tree in its desired location locally (make sure `$allbib_worktree exists` and is empty first)

    ```bash
    if find "directory_name" -prune -empty | read; then
        echo "Checking out worktree to $allbib_worktree."
        git --git-dir=$allbib_git_dir --work-tree=$allbib_worktree checkout
    else
        echo "Directory $allbib_worktree does not exist or is not empty."
    fi
    ```

-   Run step 5 above to set the config. (Skip the remote add step as remote should already be set since you just cloned.)

## Use

Once set up, your bib files will be available on GitHub to use wherever.

Anytime you make changes to the local bibliography file `all.bib`, just run the following command to automatically add those changes and commit and push to GitHub:

```bask
allbib-refresh
```

One workflow I find particularly useful is:

-   Import your allbib.git project onto Overleaf with "New Project > Import from GitHub". Then you will have this bibfile in its own Overleaf project.  You can make sure it is up to date after doing an `allbib-refresh` by selecting "Menu > GitHub" in the allbib project on Overleaf and pulling any recent changes.
-   Then in any other Overleaf project, you can "Add File > From Another Project" to import this bibfile.  Note anytime you update the file in the allbib project on Overleaf, you'll need to "refresh" the linked file in this project.
