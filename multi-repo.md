### Managing Multiple Repositories

- Every SCM System Stands on the Shoulders of its Predecessors
  - SCCS and RCS: some projects use more than one directory
  - CVS: some projects need to version-control directories, too
  - SVN: some projects can't handle a centralized bottleneck
  - git (and other DVCS systems): what's still missing?
  - Fisher's Fundamental Theorem: "Works fine as an automobile. Let's try it as a submarine!"
- Large Projects May Need Multiple Repositories
    - some projects need more than one repo
    - some repos belong to more than one project
- Three Commonly-Seen Options: `git submodule`, `git subtree`, Google's `repo`
