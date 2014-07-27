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
    - in the top-level, git can't track submodule changes
    - checking in changes is order-dependent, fragile, and labor-intensive
        0. change the submodule
        0. add changes, commit, and push
        0. add and commit at the top-level and push
Deviating from this order creates a mess.
