# github-stats-card
⭐️ *a minimal but inclusive github stats badge* ⭐️

This is a GitHub Action that generated a stats badge that you can use on your GitHub [profile page](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/customizing-your-profile/about-your-profile).

Why this stats badge generator you say? Because I noticed that people creating repos under organizations and receiving stars did not count towards their stars in most badge generation tools. And that's just a pity. So, in comes `github-stats-card`, a minimal but inclusive GitHub stats badge generator.

## Usage

The badge generator set up to works as a GitHub action. You can add it to the GH Actions workflow of your profile page as a step. Below an example workflow yaml.


```yaml
name: generate badge
on: 
  push:
  schedule:
  - cron: '0 7 * * *'

jobs:
  base-ci:
    runs-on: ubuntu-latest
    permissions:
      # Give the default GITHUB_TOKEN write permission to commit and push the
      # added or changed files to the repository.
      contents: write
    env: 
      GH_PAT: ${{ secrets.GH_PAT }}
  - uses: datarootsio/github-stats-card
    name: github-stats-card
    with:
        username: bart6114
        gh_token_stats: ${{ env.GH_PAT }}
        gh_token_commits: ${{ secrets.GITHUB_TOKEN }}
        badge_path: assets/badge.svg
        header: "👋 hi i'm"
        about: "Loves goblins, despises gnomes.\nEnjoys candlelit custard pudding."
        commit_message: Badge generated by the treadmill bunnies.
        badge_path: "assets/badge.svg"
```

If the workflow has succesfully ran, it will create a badge under `assets/badge.svg`. That you can then include in your readme via `![ain't it a beaut](assets/badge.svg)`.

## Themes

The following themes are available, see [configuration options](#configuration-options) on how to specify theme.

Want to create your own themes? Check out the `themes/` folder, it rather straightforward. PRs are very much welcome! ❤️

`theme: dark` 👇 (default theme)

![](assets/badge-dark.svg)

`theme: cool-lake` 👇 

![](assets/badge-cool-lake.svg)

`theme: neko-sleeps` 👇 

![](assets/neko-sleeps.svg)

`theme: jimmy-goes-fishing` 👇 

![](assets/badge-jimmy-goes-fishing.svg)

`theme: pad-and-paper` 👇 

![](assets/badge-pad-and-paper.svg)

`theme: retro-print` 👇 

![](assets/badge-retro-print.svg)

`theme: terminal-green` 👇 

![](assets/badge-terminal-green.svg)

`theme: tropical-sunset` 👇 

![](assets/badge-tropical-sunset.svg)

`theme: a-colibri-hums-while-the-dog-farts` 👇 

![](assets/badge-a-colibri-hums-while-the-dog-farts.svg)


## Configuration options

`gh_token_stats`: token used for fetching user stats, typically a [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) (PAT) with access to your personal and organisation repos

`gh_token_commits`: [optional] if you set this to `${{ secrets.GITHUB_TOKEN }}` it will use the github actions bot to commit the svg to the repo (if not set it will use your PAT)

`badge_path`: [optional] path of badge svg (defaults to `assets/badge.svg`)

`username`: your username

`header`: [optional] header to use for badge (defaults to: `👋 hi i'm`)

`about`: about me description to use for badge (use \n for newlines)

`commit_message`: [optional] commit message to use (defaults to `Update badge`)

`commit`: [optional] whether to commit badge to repo (defaults to `true`)

`exclude_repos`: [optional] comma separated list of repos to exclude from stats, will do regex based matching (e.g. 'datarootsio' will match all repos in dataroots, 'datarootsio/databooks' will only match a single repo)

`exclude_repos_override`: [optional] comma separated list of repos to override from exclusion list (e.g. 'datarootsio' in exclude_repos and 'datarootsio/databooks' in exclude_repos_override will ONLY include databooks in stats)

`theme`: [optional] one of `dark` (default), `cool-lake`, `jimmy-goes-fishing`, `pad-and-paper`, `retro-print`, `terminal-green`, `tropical-sunset`

## FAQ

- *Which repositories count towards my stargazers count?*

    Check out the logs of your action run. It will log which repositories are included after the exclude and override filters have been applied.

- *How do I refresh the badge?*
    
    You probably want to look into the `schedule` [event trigger](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule). It allows you to use a cron-based schedule that allows you to for example refresh your badge daily.

