# How it works

It leverages the `git am` (apply-mailbox) command to apply patches from the source repo into the desitation repo.

The patches are created using `git format-patch` and stored in `.temp` directory. Additionally, the patches are altered to ensure that the files match the new path appropriately.

---
---
---

# Warning below be dragons

From this point on, I am documenting how I came up with this approach and what other things I ran into or attempted along the way.

## Special considerations

### Original Concept

My first attempt of this was to clone a temp version of the repo, then use `git filter-branch --subdirectory-filter` to set the source directory as the root of the repo.

Then I was able add the temp repo as a remote for the destination repo. Then using `git pull temp-repo master --allow-unrelated-histories`. This was able to properly pull the files into the repo with history, but using the hold file paths.

I attempted to move the files into the correct folder sturcture for the destination. However, every attempt to move the file was losing the history of the files either immediately or when it was pulled into the new repo.

Additionally, since this required a clone it took far too long to be sustainable.

### Other Attempts

I played around with a few other features in `git filter-branch` to attempt to re-write and move the files using the `--index-filter` flag, but this turned out to be too confusing and convoluted to work. It was also requiring a separate clone which as stated above was slow and the actions needed to perform were too dangerous to run in the already cloned copy.

### Lerna Approach

The final approach and working solution was to emulate was Lerna was doing to [import packages into mono repos](https://github.com/lerna/lerna/blob/a7ad9b60d27b390fde21fd2837f2d97320c4603e/commands/import/index.js#L163-L171). Reviewing their import command led me to find `git format-patch` to pull the collections of patches necessary to re-create the files.
To avoid creating any files outside the source directory I appended it to the command.
Also since the repo could be fairly large I pull the commit history of the directory to only pull patches for range of commits from creation (minus one to include the patch that created the files) to latest. To handle cases where the creation of the directory was the first commit in the repo, I fall back to a special sha that git uses to indicate the empty repo (`4b825dc642cb6eb9a060e54bf8d69288fbee4904`, thanks Jamie!).

Once we had the list of patches, it would have applied them based on the old file structure. To ensure they were properly placed into the destination directory I regex replaced with the patch files to update references from the old path to the new path. Using the `--src-prefix` and `--dest-prefix` on the `format-patch` command allowed for this to simpler (thank again [lerna](https://github.com/lerna/lerna/blob/a7ad9b60d27b390fde21fd2837f2d97320c4603e/commands/import/index.js#L168-L170))

I ran into a case where applying the patches failed due to a file rename, so I extended the regex to include the necessary `rename [to|from]` replacements.

If you were to stop the process or leave the files to review them, you will notice it does still include references to the old path. These were left to avoid over altering the file since it was working as expected without replacing them.
