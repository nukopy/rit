# rit

Implementation of Git in Rust

## Environment

- Rust 1.69.0

## About Git

### Directory structure of `.git` when you execute `git init`

```sh
$ git init
$ tree -L 2 .git
.git
├── HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   ├── push-to-checkout.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags
```

### Essential files and directories of Git

Essential files and directories of Git in `.git` directory are as follows:

- `HEAD`
  - A file that points to the current branch
- `index`
  - A file that stores the information of the staging area
- `objects`
  - A directory that stores all contents (files) managed by Git
- `refs`
  - A directory that stores pointers (branch) that point to commit objects in contents

#### `HEAD`

```sh
$ cat .git/HEAD
ref: refs/heads/main
```

#### `index`

```sh
$ cat .git/index
DIRCdv9�!Z�0dv9�!Z��Ul���v� (�)�b��W\EX�]�      README.mdTREE1 0
_y����S��e�(��f���EC�ո�p4*I� �r�%
```

#### `objects`

```sh
$ tree .git/objects
.git/objects
├── 29
│   └── bfa74fe2e6b2bcd512b81e1e71a8dcb1ef0f0d
├── 5f
│   └── 79def7d5c15303a8a565fe28fb9d66dceaec98
├── 8a
│   └── 122028a529e462811a84575c12014558a65dac
├── info
└── pack
```

#### `refs`

```sh
$ tree .git/refs
.git/refs
├── heads
│   └── main
├── remotes
│   └── origin
│       └── main
└── tags

$ cat .git/refs/heads/main
29bfa74fe2e6b2bcd512b81e1e71a8dcb1ef0f0d

$ cat .git/refs/remotes/origin/main
29bfa74fe2e6b2bcd512b81e1e71a8dcb1ef0f0d
```

### Get a content with `git cat-file -p`

```sh
# commit
$ echo "# rit" > README.md
$ git add README.md
$ git commit -m "Initial commit"

# check up commit object  and get a hash value of tree object
$ git cat-file -p HEAD
tree 5f79def7d5c15303a8a565fe28fb9d66dceaec98
author nukopy <nukopy@gmail.com> 1685469635 +0900
committer nukopy <nukopy@gmail.com> 1685469635 +0900

Initial commit

$ git cat-file -p 5f79def7d5c15303a8a565fe28fb9d66dceaec98
100644 blob 8a122028a529e462811a84575c12014558a65dac    README.md
```

## References

- [(Zenn, 2020/11) Rust でつくる Git 入門](https://zenn.dev/uzimaru0000/books/impl-git-in-rust)
- official documentation of Git
  - [10.1 Git の内側 - 配管（Plumbing）と磁器（Porcelain）](https://git-scm.com/book/ja/v2/Git%E3%81%AE%E5%86%85%E5%81%B4-%E9%85%8D%E7%AE%A1%EF%BC%88Plumbing%EF%BC%89%E3%81%A8%E7%A3%81%E5%99%A8%EF%BC%88Porcelain%EF%BC%89)
  - [10.2 Git の内側 - Git オブジェクト](https://git-scm.com/book/ja/v2/Git%E3%81%AE%E5%86%85%E5%81%B4-Git%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)
- [(2015/09) mercari engineering - Git のつくりかた](https://engineering.mercari.com/blog/entry/2015-09-14-175300/)
- [(Slideshare, 2015/08) Git のつくりかた YAPC::Asia 2015 @DQNEO](https://www.slideshare.net/DQNEO/git-yapcasia-2015-dqneo)
- [(Zenn, 2021/08)Git のインデックスの中身](https://zenn.dev/kaityo256/articles/inside_the_index)
- [github.com/git/git](https://github.com/git/git)
  - Git Source Code Mirror
