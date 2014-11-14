WARNING WARNING

we have forked this from the upstream repo because it is lacking certain features for dealing with large numbers of repositories.  This has been updated to make like more tolerable.

	http://github.com/wotio/wot-github


====


[![browser support](https://ci.testling.com/darvin/github.png)](https://ci.testling.com/darvin/github)


[![Build Status](https://travis-ci.org/darvin/github.png?branch=master)](https://travis-ci.org/darvin/github)

# Github.js

Github.js provides a minimal higher-level wrapper around git's [plumbing commands](http://git-scm.com/book/en/Git-Internals-Plumbing-and-Porcelain), exposing an API for manipulating GitHub repositories on the file level. It is being developed in the context of [Prose](http://prose.io), a content editor for GitHub.

This repo is now officially maintained by [DevelopmentSeed](https://github.com/developmentseed), the people behind [Prose.io](http://prose.io).

## Usage

Create a Github instance.

```js
var github = new Github({
  username: "YOU_USER",
  password: "YOUR_PASSWORD",
  auth: "basic"
});
```

Or if you prefer OAuth, it looks like this:

```js
var github = new Github({
  token: "OAUTH_TOKEN"
  auth: "oauth"
});
```

## Repository API


```js
var repo = github.getRepo(username, reponame);
```

Show repository information

```js
repo.show(function(err, repo) {});
```

Get contents at a particular path.

```js
repo.contents("path/to/dir", function(err, contents) {});
```

Fork repository. This operation runs asynchronously. You may want to poll for `repo.contents` until the forked repo is ready.

```js
repo.fork(function(err) {});
```

Create Pull Request.

```js
var pull = {
  title: message,
  body: "This pull request has been automatically generated by Prose.io.",
  base: "gh-pages",
  head: "michael" + ":" + "prose-patch",
};
repo.createPullRequest(pull, function(err, pullRequest) {});
```


Retrieve all available branches (aka heads) of a repository.

```js
repo.listBranches(function(err, branches) {});
```

Store contents at a certain path, where files that don't yet exist are created on the fly.

```js
repo.write('master', 'path/to/file', 'YOUR_NEW_CONTENTS', 'YOUR_COMMIT_MESSAGE', function(err) {});
```

Not only can you can write files, you can of course read them.

```js
repo.read('master', 'path/to/file', function(err, data) {});
```

Move a file from A to B.

```js
repo.move('master', 'path/to/file', 'path/to/new_file', function(err) {});
```

Remove a file.

```js
repo.remove('master', 'path/to/file', function(err) {});
```

Exploring files of a repository is easy too by accessing the top level tree object.

```js
repo.getTree('master', function(err, tree) {});
```

If you want to access all blobs and trees recursively, you can add `?recursive=true`.

```js
repo.getTree('master?recursive=true', function(err, tree) {});
```

Given a filepath, retrieve the reference blob or tree sha.

```js
repo.getSha('master', '/path/to/file', function(err, sha) {});
```

For a given reference, get the corresponding commit sha.

```js
repo.getRef('heads/master', function(err, sha) {});
```

Create a new reference.

```js
var refSpec = {
  "ref": "refs/heads/my-new-branch-name",
  "sha": "827efc6d56897b048c772eb4087f854f46256132"
};
repo.createRef(refSpec, function(err) {});
```

Delete a reference.

```js
repo.deleteRef('heads/gh-pages', function(err) {});
```


## User API


```js
var user = github.getUser();
```

List all repositories of the authenticated user, including private repositories and repositories in which the user is a collaborator and not an owner.

```js
user.repos(function(err, repos) {});
```

List organizations the autenticated user belongs to.

```js
user.orgs(function(err, orgs) {});
```

List authenticated user's gists.

```js
user.gists(username, function(err, gists) {});
```

Show user information for a particular username. Also works for organizations.

```js
user.show(username, function(err, user) {});
```

List public repositories for a particular user.

```js
user.userRepos(username, function(err, repos) {});
```

List repositories for a particular organization. Includes private repositories if you are authorized.

```js
user.orgRepos(orgname, function(err, repos) {});
```

List all gists of a particular user. If username is ommitted gists of the current authenticated user are returned.

```js
user.userGists(username, function(err, gists) {});
```

## Gist API

```js
var gist = github.getGist(3165654);
```

Read the contents of a Gist.

```js
gist.read(function(err, gist) {

});
```

Updating the contents of a Git. Please consult the documentation on [GitHub](http://developer.github.com/v3/gists/). 

```js
var delta = {
  "description": "the description for this gist",
  "files": {
    "file1.txt": {
      "content": "updated file contents"
    },
    "old_name.txt": {
      "filename": "new_name.txt",
      "content": "modified contents"
    },
    "new_file.txt": {
      "content": "a new file"
    },
    "delete_this_file.txt": null
  }
};

gist.update(delta, function(err, gist) {
  
});
```


## Tests

Github.js is automatically™ tested by the users of [Prose](http://prose.io). Because of that, we decided to save some time by not maintaining a test suite. Yes, you heard right. :) However, you can still consider it stable since it is used in production.

##Setup

Github.js has the following dependencies:

- Underscore
- Base64 (for basic auth). You can leave this if you are not using basic auth.

Include these before github.js :

```
<script src="lib/underscore-min.js">
<script src="lib/base64.js">
<script src="github.js">
```

## Change Log


### 0.7.X

Switched to a native `request` implementation (thanks @mattpass). Adds support for GitHub gists, forks and pull requests.

### 0.6.X

Adds support for organizations and fixes an encoding issue.

### 0.5.X

Smart caching of latest commit sha. 

### 0.4.X

Added support for [OAuth](http://developer.github.com/v3/oauth/).

### 0.3.X

Support for Moving and removing files.

### 0.2.X

Consider commit messages.

### 0.1.X

Initial version.
