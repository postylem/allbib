This is a way of keeping a copy of all my references in a bib file in GitHub.
Then I can use that in Overleaf (this is similar to the features they offer for Zotero or Mendeley bibliography synching, but does not require having to have a paid subscription to overleaf :).

## Setup:

Add the following to your zsh config file (located by default at `~/.zshrc`):

```bash
# for my allbib.git repo, which just tracks the dir where my all.bib file is
alias allbib-git='git --git-dir=$HOME/allbib.git --work-tree=$HOME/Library/texmf/bibtex/bib/'
alias allbib-refresh='allbib-git add $HOME/Library/texmf/bibtex/bib/ && allbib-git commit -m "routine update" && allbib-git push'
```
