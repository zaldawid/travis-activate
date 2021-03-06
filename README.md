# travis-activate.py 

If you have a GitHub project with an ever-growing number of repos, and
you want to automatically activate those repos for Travis-CI builds
without having to manually click all the switches on their web site,
then this script is for you.

## Installation


1) If you haven't already done this, you'll first need to `pip install
requests` for a necessary library.

2) You need to get a "token" for Travis-CI, which isn't the same thing
as the "token" that Travis-CI displays when you log into it. (Why?
No idea.)

3) First, get a GitHub token with all the "Repo" privileges. You do
this on the GitHub website
[(instructions)](https://github.com/blog/1509-personal-api-tokens). 

4) Install the [httpie](https://httpie.org/) command-line tool.

5) Run this one-liner, or something like it with your favorite
  alternative tool, telling Travis your
  GitHub API token and returning you a Travis-API token. (Replace
  "XXXXX" with your GitHub API token.)

```
http POST https://api.travis-ci.com/auth/github github_token=XXXXX
```

6) Now you'll have a Travis API token. Edit the `travis-activate.py`
script and paste it at the top where the code says `travisToken = `.
(If you're doing this for multiple repos, you could presumably modify
this script to take the token as an external argument. We're more
interested in having a script that "just works" without any
environmental dependencies.)

7) Edit the `githubProject` string to reflect your
   project's name (e.g., for `https://github.com/RiceComp215`, the
   project name is `RiceComp215`). You should also write a suitable
   regular expression in `repoRegex`, specifying
   which repos you want to include. Non-matching repos will be
   ignored.


## Usage

Now you can simply run `python travis-activate.py` and
it will both activate any previously inactive repos (matching the
regex) and will request a rebuild for them. Easy! Run it from a cron script? Sure!
Note that this script *requests* that Travis synchronize itself with
GitHub, but this request appears to be asynchronous, so a single run
of this script might still miss some recently created repositories.
