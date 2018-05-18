![icon](mergeable.png)

# The Mergeable Bot

A GitHub App that validates pull requests for mergeability based on [rule-sets](#configuration). Make your pull requests mergeable only when the:

- **Title** does not contain the terms `wip`, `dnm`, `exp`, or `poc`.

- **Label** contains the terms `work in progress`, `do not merge`, `experimental` or `proof of concept`

- **Milestone** on the pull request matches with what is configured.

- **Approvals** have at least 1 approved review.

- **Description** of the pull request is not empty.

The above is the default. More options and rule-sets are available through (#configuration).

![Screenshot](https://raw.githubusercontent.com/jusx/mergeable/5d9c9cab357b12b84af62044ac46648d9fca84c4/screenshot.gif)

<p style="text-align:center">
<a href="https://github.com/apps/mergeable" style="padding:10px;background-image: linear-gradient(-180deg, #34d058 0%, #28a745 90%);background-color: #28a745;color:#fff;border:1px solid #111;border-radius: 0.25em;">Install Mergeable for Free</a>
</p>

<p style="text-align:center">Encourage **consistency** on your github pull request workflow.</p>

## Configuration
Mergeable is fully configurable. Configuration of the ruleset can be done in two ways. Simple and advanced. You can configure mergeable by creating a `.github/mergeable.yml` file in your repository.

If the yml file doesn't exist, the bot will default to the following ruleset:

```yml
mergeable:
  # PR must not have any label that has the following terms to be mergeable
  label: 'work in progress|do not merge|experimental|proof of concept'

  # PR must not have any of the following terms in the title. i.e. (wip) My PR Title
  title: 'wip|dnm|exp|poc'

  # Minimum of 1 review approval is needed.
  approvals: 1
```

A simple configuration would be as follows:

```yml
mergeable:
  # Minimum of 5 approvals is needed.
  approvals: 5

  # Regular expression. In this example, whenever a PR has a label with the word 'wip'
  label: 'wip|do not merge|experimental'

  # Regular expression to be tested on the title. Not mergeable when true.  
  title: 'wip'

  # Only mergeable when milestone is as specified below.
  milestone: 'version 1'

  # exclude any of the mergeable validation above. A comma separated list. For example, the following will exclude validations for approvals and label.
  exclude: 'approvals, label'
```

However you may want to have more advanced rulesets:

```YML

  # implemented!
  title:
    must_include:
      regex: `^\\(feat\\)|^\\(doc\\)|^\\(fix\\)`
      message: `Title must have prefixes for the following: (feat), (doc), (fix)`
    must_exclude:
      regex: 'wip'
      message: 'This PR is work in progress.'
    begins_with:
      match: '(feat)|(doc)|(fix)'
      message: 'Custom message...'
    ends_with:
      match: '(feat)|(doc)|(fix)'
      message: 'Custom message...'

  # not implemented yet.    
  label:
    must_include:
      regex: `^\\(feat\\)|^\\(doc\\)|^\\(fix\\)`
      message: `Title must have prefixes for the following: (feat), (doc), (fix)`
    must_exclude:
      regex: 'wip'
      message: 'Custom message. This PR is work in progress.'
    begins_with:
      match: '(feat)|(doc)|(fix)'
      message: 'Come message...'
    ends_with:
      match: '(feat)|(doc)|(fix)'
      message: 'Come message...'  

  # not implemented yet.    
  milestone:
    must_include:
      regex: `Release 1`
      message: `Custom message...`
    must_exclude:
      regex: 'jibberish'
      message: 'Custom message...'
    begins_with:
      match: 'Release'
      message: 'Custom message...'
    ends_with:
      match: ''
      message: 'Custom message...'  

  # not implemented yet.    
  approvals:
    min: 5
      message: 'Custom message...'
    required:
      reviewers: [ user1, user2 ]   # list of gh usernames
      message: 'Custom message...'


  # not implemented yet.  
  description:
    no_empty:
      enabled: false
      message: 'Custom message...'
    must_include:
      regex: 'feat'
      message: 'Custom message...'
    must_exclude:
      regex: 'DO NOT MERGE'
      message:

  # not implemented yet.      
  assignee:
    min: 1
    max: 1
    message: 'Custom message...'
```    


## Usage

### Install the app
1. [Install](https://github.com/apps/mergeable) the Mergeable GitHub App.
2. [Configure](#configuration) Mergeable or do nothing else and just go with the default.
3. Ensure branch is protected and [Require status checks to pass before merging](https://help.github.com/articles/enabling-required-status-checks/) is checked.

### Deploy your own

If you would like to run your own instance of this plugin, you can do so by forking this repo and deploying it to your own servers.

[Create a GitHub App](https://github.com/settings/apps/new) and configure the permissions & events with the following settings:

- Repository metadata - **Read Only**
- Commit Statuses - **Read & Write**
- Pull requests - **Read Only**
- Issues - **Read Only**
- Single File - **Read-only**
  - Path: `.github/mergeable.yml`

And subscription to the following events:
- [x] Pull request
- [x] Pull request review comment
- [x] Pull request review
- [x] Issues

## Contributions
On [Github](https://github.com/jusx/mergeable): [Contribute](https://github.com/jusx/mergeable/blob/master/CONTRIBUTING.md) by creating a pull request or create a [new issue](https://github.com/jusx/mergeable/issues) to request for features.


---
[![CircleCI](https://circleci.com/gh/jusx/mergeable.svg?style=shield)](https://circleci.com/gh/jusx/mergeable) & built with [probot](https://github.com/probot/probot).
