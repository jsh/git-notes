### Finer-Grained Access Control

- Why Use Access-Control Lists (ACLs)?
  - Let organizations, project owners, and source-code managers provide limited access 
  - Example: New users get read access, experienced developers can commit, owners can tag
- Early ACL Models for Git Are Centralized
  - `gitosis`
  - `gitolite`, the successor to `gitosis`
  - *quis custodiet custodes?*
All access is via ssh-keys, but who administers the keys?
- GitHub's Model is Decentralized, by Repo Owner
  - Users manage their own ssh keys.
  - Users manage their own repos, but can designate "teams," and "collaborators," or make repos private.
- Google's `gerrit` Offers a More Complex, Decentralized Model
  - Projects have very-fine-grained permissions.
  - Groups of projects can inherit ACLs and subclass them.
  - Projects and project groups can have designated owners that manage their ACLs and users.
  - Users manage their own ssh keys.
