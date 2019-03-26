# Git

## Commits

* Modify commits using `git rebase -i '<commit hash>^'`
    * `reword` to edit commit message
    * `edit` to edit commit
        * commit corrections with `git commit --amend --no-edit`
        * and then `git rebase --continue`
* If already pushed, `git commit <remote> <branch> -f`
* SO:
    * https://stackoverflow.com/questions/2719579/how-to-add-a-changed-file-to-an-older-not-last-commit-in-git
    * https://stackoverflow.com/questions/1186535/how-to-modify-a-specified-commit

## Tags

* List tags with message: `git tag -n`
* Create annotated tag and push:
    * `git tag -a -m <tag message> <tag name> [commit hash]`
    * `git push --tags` or `git push <remote> <tag name>`
* Remove tag that is already pushed:
    * remove tag on remote with `git push origin :refs/tags/<tag name>`, `:` stands for `--delete`
    * replace tag locally with `git tag -af <tag annotation> <commit hash>`, `-f` means replace existing tag.
    * force push tags with `git push --tags -f`

## Misc

* Show changes in latest commit: `git show`
* `git log --oneline --graph --all`
* Choose which part of a file to stage: `git add -p`
    * Manually split hunk with `e`: https://stackoverflow.com/a/37769129/5536516
