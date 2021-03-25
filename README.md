# [Neuro Flow](https://github.com/neuro-inc/neuro-flow) git checkout action

This batch mode actions checkouts are repository to provided volume, to 
make it available to your workflow. Fetches only single commit by default.

## Quick example:

```
tasks:
  - id: checkout
    action: gh:neuro-actions/checkout@v1
    args:
      clone_uri: https://github.com/neuro-actions/checkout.git
      checkout_to: "storage:"
      checkout_subpath: dir/to/clone/into
```

## Arguments:

### `clone_uri`

The git repository url to clone. Both `https://` and `git@` are supported.

##### Example
```
args:
  clone_uri: https://github.com/neuro-actions/checkout.git
```

Note that you should provide a `ssh_key_secret` when you using ssh clone (`git@...`). 

### `clone_depth`

Number of commits to fetch. `"1"` by default, which means to clone only single commit.
To clone all repo, set `"0"`. 

##### Example
```
args:
  clone_depth: "10"
```

### `ref`

A name of branch, git tag or a commit SHA to fetch. Will use remote default branch 
(more precisly, remote HEAD) by default.

##### Example
```
args:
  ref: main
```

### `ssh_key_secret`

A uri of [neuro platform secret](https://docs.neu.ro/core/secrets) that contains
ssh private key to use for cloning.

If you are on unix machine with default configuration, you can upload your current
private key by this command:

```bash
neuro secret add git_ssh_private_key @~/.ssh/id_rsa
```

##### Example
```
args:
  ssh_key_secret: secret:git_ssh_private_key
```

### `checkout_to`

A uri of storage folder or disk to use as volume for checkout. When using 
disk, you can use `checkout_subpath` to specify directory to checkout.

Note that for `storage:` volumes, the corresponding directory should exist.
If you want to automatically create subdirectory, use `checkout_subpath`.

##### Examples
Storage based volume
```
args:
  checkout_to: storage:dir/to/checkout
```

Disk based volume
```
args:
  checkout_to: disk:disk_name_or_id
```

### `checkout_subpath`

A relative path under `checkout_to` to use as directory to clone to.
If such folder does not exits, it will be automatically created.

##### Example
```
args:
  checkout_subpath: store/repo_content/here
```

### `erase_subpath`

Enables purging of directory specified by `checkout_subpath`. Use with caution,
this is same as running `rm -rf checkout_subpath` command!
To enable, set input to `"true"`.

##### Example
```
args:
  erase_subpath: "true"
```

## Outputs:

### `head_sha`

A SHA of the commit the is under current HEAD in cloned repository.
This output main goal is to make this action cache-friendly -
even the repository did not changed, that output will be the same
and depending action can safely use the cache.



 