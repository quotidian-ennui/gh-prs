# gh-prs

Just a bunch of wrappers around existing functionality I have as aliases that manipulate PRs en-masse

## Commands

- `gh prs retry [pr...]`
  - retries all the failed jobs for each pr supplied
- `gh prs label <label> [pr...]`
  - simply executes `gh pr edit --add-label <label> <pr>` for each prsupplied
- `gh prs remove-label <label> [pr...]`
  - simply executes `gh pr edit --remove-label <label> <pr>` for each pr supplied
- `gh prs rebase [pr...]`
  - equivalent to the alias `rebase: '!for i in $@; do gh pr update-branch $i --rebase; done'`
- `gh prs approve [pr...]`
  - equivalent to the alias `approve: '!for i in $@; do gh pr review --approve $i; done'`
- `gh prs bot-merge [pr...]`
  - equivalent to the alias `bot-merge: '!for i in $@; do gh approve $i; gh squash-merge $i --use-default-msg; done'`
- `gh prs dependabot rebase`
  - equivalent to the alias `dbot-rebase: '!gh pr list -A "app/dependabot" --json number --jq ".[].number" | xargs -r -IID gh pr comment --body "@dependabot rebase" ID'`
- `gh prs dependabot recreate`
  - equivalent to the alias `dbot-recreate: '!gh pr list -A "app/dependabot" --json number --jq ".[].number" | xargs -r -IID gh pr comment --body "@dependabot recreate" ID'`


### Notes

> - if no `<pr>` is passed then it tries to work out of there is a PR associated with the branch, and always terminates with a warning in the branch is the default branch.
> - `<pr>` can be a number or a URL.

### Help

```
bsh ❯ gh prs help

Usage: gh prs <command> [options] [pr...]

  retry               : retry all failed jobs for each <pr>
  label               : add a label to each <pr>
  remove-label        : remove a label from each <pr> (aliased as 'rm-label')
  rebase              : update each <pr> branch via rebase
  approve             : approve each <pr>
  bot-merge           : approve and squash-merge each <pr>
  dependabot          : trigger dependabot sub-commands on all open dependabot PRs (aliased as 'dbot')
                           rebase   : post @dependabot rebase
                           recreate : post @dependabot recreate
  help                : show this help message

If no <pr> is supplied the PR for the current branch is used.
Terminates with a warning if the current branch is the default branch.

<pr> can be a number or a URL.
```


## License

See [LICENSE](./LICENSE)