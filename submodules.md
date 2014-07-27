## git submodule

### First, let's show them off:

        #!/bin/bash -eu 
        # create a base repo
        git-scratch; popd
        barerepos=$PWD/repos.d

        # make three bare repos: top-level and two subrepos
        git clone --bare git-scratch.d $barerepos/top.git
        git clone --bare git-scratch.d $barerepos/a.git
        git clone --bare git-scratch.d $barerepos/b.git

        # do something to help tell them apart

        for repo in top a b; do
        git clone file://$barerepos/$repo.git
        pushd $repo
            echo "This is repo '$repo'" >> README.$repo
            git commit -am"Identify the repo"
            git push
        popd
        done

        # Now let's connect them
        pushd top
        git submodule add file://$barerepos/a.git
        git submodule add file://$barerepos/b.git

        git status
        git commit -m"Commit the subrepos"
        git push

        # and try it out
        popd

        rm -rf top a b
        git clone file://$barerepos/top.git
        pushd top
            git submodule update --init
            git config --get-regexp submodule
            tree 
        popd

        # It works!

### What could possibly go wrong?

- It's confusing.
  - `git submodule init` creates empty directories
  - top/{a,b}/.git are files, not directories
  - even after you figure it out, your co-workers will get confused
- They're indepenent repos
  - at the top-level, git can't track submodule changes
  - there's no convenient way to tie commits in one submodule to the others
  - Jenkins is not submodule-aware, only reports changes to top-level.
- Submodules are checked out as detached HEADs -- `(no branch)`
  - bug or feature?
  - checking out or committing top-level modules gets *specific, fixed versions* of every submodule,
    - great for releases
    - bad for development
- Making changes is order-dependent, fragile, and labor-intensive
  0. checkout the correct branch of each submodule you need to change
  0. change the submodule, add changes, commit, and push
  0. add submodules and commit at the top-level and push

  No good workflow-tool support, and **deviating from this order creates a mess**.
  
