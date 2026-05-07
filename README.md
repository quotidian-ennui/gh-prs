# gh-prs

Just a bunch of wrappers around existing functionality I have as aliases that manipulate PRs en-masse

## Commands

- `gh prs retry [pr...]`
  - retries all the failed jobs for each pr supplied
- `gh prs resolve-all-reviews [pr...]`
  - Because you've foolishly asked some AI bot to review your PR, and there is a lot of meaningless slop that is causing you a bit of frustration; why not just resolve all that shit already.
- `gh prs lazy-merge [pr...]`
  - This is the equivalent of doing `gh resolve-all-reviews` and `gh merge-train` for every PR.
  - 💥💥 Like Homer's drinking bird, you're super lazy. You're an awful person, and you don't deserve good things.
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
bsh ❯ gh prs --help

Usage: gh prs <command> [options] [pr...]

  retry               : retry all failed jobs for each <pr>
  label               : add a label to each <pr>
  remove-label        : remove a label from each <pr> (aliased as 'rm-label')
  rebase              : update each <pr> branch via rebase
  approve             : approve each <pr>
  bot-merge           : approve and squash-merge each <pr>
  resolve-all-reviews : resolve all open review threads for each <pr> (aliased as 'resolve')
  lazy-merge          : resolve all review threads then merge-train each <pr>
  dependabot          : trigger dependabot sub-commands on all open dependabot PRs (aliased as 'dbot')
                           rebase   : post @dependabot rebase
                           recreate : post @dependabot recreate
  help                : show this help message

If no <pr> is supplied the PR for the current branch is used.
Terminates with a warning if the current branch is the default branch.

<pr> can be a number or a URL.

gh extensions that are expected:
  gh squash-merge  : used by bot-merge (and merge-train)
  gh merge-train   : used by lazy-merge
```


## License

See [LICENSE](./LICENSE)