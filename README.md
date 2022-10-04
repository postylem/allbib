This is a way of keeping a copy of all my references in a bib file in GitHub.
Then I can use that in Overleaf (this is similar to the features they offer for Zotero or Mendeley bibliography synching, but does not require having to have a paid subscription to overleaf :). 

## Setup:


1. Add the following lines to your zsh config file (located by default at `~/.zshrc`).  The first two commands set variables for the location of the work tree, and the git dir.  I choose to put them in the following locations:
  ```zsh
  # Set locations of allbib work tree and git dir
  allbib-work-tree-dir=$HOME/Library/texmf/bibtex/bib/
  allbib-git-dir=$HOME/allbib.git

  # for my allbib.git repo, which just tracks the dir where my all.bib file is
  alias allbib-git='git --git-dir=$allbib-git-dir --work-tree=$allbib-work-tree-dir'
  alias allbib-refresh='allbib-git add $allbib-work-tree-dir && allbib-git commit -m "routine update" && allbib-git push'
  ```

2. Then close your terminal and open a new terminal (or reload the zsh config file), so those variables are available.

3. Then, initialize a bare git repository to track your bibfiles, located in the place you chose above.
  ```zsh
  git init --bare $allbib-git-dir
  ```

4. Add a bib file to be tracked.  Say you have a file `all.bib` in the allbib-work-tree-dir you specified above.  Add it and commit like this:
```zsh
  allbib-git add all.bib
  allbib-git commit -m 'added all.bib'
```

5. Make Git not show all the untracked files, then add the remote (make the new repo called allbib.git on github first)
```bash
allbib-git config status.showUntrackedFiles no
allbib-git remote add origin https://github.com/$GITHUB_USERNAME/allbib.git
```


## Use

Once set up, your bib files will be available on GitHub to use wherever.

Anytime you make changes to the local bibliography file `all.bib`, just run the following command to automatically add those changes and commit and push to GitHub:

```zsh
allbib-refresh
```

One use-case I find particularly useful is the following: 

- Import your allbib.git project onto Overleaf with "New Project > Import from GitHub". Then you will have this bibfile in its own Overleaf project.  You can make sure it is up to date after doing an `allbib-refresh` by selecting "Menu > GitHub" in the allbib project on Overleaf and pulling any recent changes.
- Then in any other Overleaf project, you can "Add File > From Another Project" to import this bibfile.  Note anytime you update the file in the allbib project on Overleaf, you'll need to "refresh" the linked file in this project.
