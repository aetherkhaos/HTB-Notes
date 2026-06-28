## Sparse Checkout and pulling specific dirs and files

```
# Clone without checking out any files
git clone --no-checkout --filter=blob:none git@github.com:aetherkhaos/tool-backup.git
cd tool-backup

# Enable sparse checkout
git sparse-checkout init --cone

# Set which directories you want
git sparse-checkout set dir1 dir2 dir3

# Now pull them down
git checkout main

# Adding additional directories
git sparse-checkout add dir4 dir5

# adding directories to the repo that are not on the sparse list
git add --sparse <dir>

# pulling a repos dir structure
git clone --no-checkout --filter=blob:none git@github.com:<private_repo>.git
cd into the cloned repo
git ls-tree --name-only HEAD

# pulling specific files from a public repo
curl -O https://raw.githubusercontent.com/<user>/<repo>/main/<file>
wget https://raw.githubusercontent.com/<user>/<repo>/main/<file>

git clone --no-checkout --filter=blob:none https://github.com/<user>/<repo>.git
cd <repo>
git sparse-checkout set --no-cone README.md
git checkout main


# # pulling specific files from a private repo
curl -H "Authorization: token ghp_yourtokenhere" \
  -H "Accept: application/vnd.github.v3.raw" \
  -O -L "https://api.github.com/repos/<user>/<repo>/contents/README.md"

git clone --no-checkout --filter=blob:none git@github.com:<user>/<repo>.git
cd <repo>
git sparse-checkout set --no-cone README.md
git checkout main
```
