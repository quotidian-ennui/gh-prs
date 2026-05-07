# gh-prs

Just a bunch of wrappers around existing functionality some of which I already had as aliases. If you end up managing a lot of repositories that end up having the same "PRs" raised on them (oooh, look it's yet another release of dependency X that I tend to import in all my code; you know which ones they are).

## Commands

- `gh prs retry [pr...]`
  - retries all the failed jobs for each pr supplied
- `gh prs resolve-all-reviews [pr...]`
  - Because you've foolishly asked some AI bot to review your PR, and there is a lot of meaningless slop that is causing you a bit of frustration; why not just resolve all that shit already.
- `gh prs lazy-merge [pr...]`
  - This is the equivalent of doing `gh prs resolve-all-reviews` and then `gh merge-train` for every PR.
  - Like Homer's drinking bird, you're super lazy. What a life you have; and in some ways; maybe you don't deserve nice things; yet here we are (being lazy isn't bad, it is after all the mother of all invention).
  - ![drinking-bird](https://media.tenor.com/SFmuIQmkIkQAAAAM/catfix-funny.gif)
- `gh prs label <label> [pr...]`
  - simply executes `gh pr edit --add-label <label> <pr>` for each pr supplied. It will try to check if that label exists before doing something.
- `gh prs remove-label <label> [pr...]`
  - simply executes `gh pr edit --remove-label <label> <pr>` for each pr supplied.  It will try to check if that label exists before doing something.
- `gh prs rebase [pr...]`
  - It's fewer keystrokes than the alias `rebase: '!for i in $@; do gh pr update-branch $i --rebase; done'`
- `gh prs approve [pr...]`
  - It's fewer keystrokes than the alias `approve: '!for i in $@; do gh pr review --approve $i; done'`
- `gh prs bot-merge [pr...]`
  - It's fewer keystrokes than the alias `bot-merge: '!for i in $@; do gh approve $i; gh squash-merge $i --use-default-msg; done'`
- `gh prs dependabot rebase`
  - equivalent to the alias `dbot-rebase: '!gh pr list -A "app/dependabot" --json number --jq ".[].number" | xargs -r -IID gh pr comment --body "@dependabot rebase" ID'`
- `gh prs dependabot recreate`
  - equivalent to the alias `dbot-recreate: '!gh pr list -A "app/dependabot" --json number --jq ".[].number" | xargs -r -IID gh pr comment --body "@dependabot recreate" ID'`


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