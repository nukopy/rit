# rit

Implementation of Git in Rust

## Environment

- Rust 1.69.0

## Note

### About Git

#### Directory structure of `.git` when you execute `git init`

```sh
$ git init
$ tree -L 2 .git
.git
├── HEAD
├── config
├── description
├── hooks
├── info
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags
```

#### Essential files and directories of Git

Essential files and directories of Git in `.git` directory are as follows:

- `objects`
  - A directory that stores all contents (text / binary files) managed by Git
- `HEAD`
  - A text file that points to the current branch. A content of `HEAD` contains file path to files in `refs` directory.
- `refs`
  - A directory that stores pointers (branch / tag) that point to commit objects in contents. A content of `refs` directory is as follows:
    - `heads`
      - A directory that stores pointers (branch) that point to commit objects in contents
    - `tags`
      - A directory that stores pointers (tag) that point to commit objects in contents
- `index`
  - A file that stores the information of the staging area

<!--

#### `HEAD`

```sh
$ cat .git/HEAD
ref: refs/heads/main
```

#### `index`

```sh
# create index file & blob object (staging with `git add`)
$ git add README.md
$ git add .gitignore

# show content in index file
$ cat .git/index
DIRCdvM�0]�dvM�0]��B����@�'�hz#.�L���
.gitignoredvJ�$�hSdvJ�$�h�Ul���k��?xpy�B\̀���    README.mdTREE-1 0
Y��I�T/5�`�}��Ga�

$ git ls-files --stage
100644 7f08e740fa27a1687a232ed74c0e16971baf99ad 0       .gitignore
100644 99048c3f7870798e427f005ccd8083dd14d91b48 0       README.md

# show type of object
$ git cat-file -t 99048c3f7870798e427f005ccd8083dd14d91b48
blob

# show content of object (output is this README.md)
$ git cat-file -p 99048c3f7870798e427f005ccd8083dd14d91b48
# --- output of git cat-file ---
# rit

Implementation of Git in Rust

## Environment

- Rust 1.69.0

## About Git

...
# ------------------------------

# show hash value of object
$ git hash-object README.md
94ed94db4e9cde081610b6455660501a27bdbcf1

# show .git/objects
$ tree .git/objects
.git/objects
├── 29
│   └── bfa74fe2e6b2bcd512b81e1e71a8dcb1ef0f0d
├── 5f
│   └── 79def7d5c15303a8a565fe28fb9d66dceaec98
├── 7f
│   └── 08e740fa27a1687a232ed74c0e16971baf99ad
├── 8a
│   └── 122028a529e462811a84575c12014558a65dac
├── 99
│   └── 048c3f7870798e427f005ccd8083dd14d91b48
├── info
└── pack

8 directories, 5 files

# unstage .gitignore & commit README.md
$ git restore --staged .gitignore

# show .git/objects (not changed!)
$ tree .git/objects
.git/objects
├── 29
│   └── bfa74fe2e6b2bcd512b81e1e71a8dcb1ef0f0d
├── 5f
│   └── 79def7d5c15303a8a565fe28fb9d66dceaec98
├── 7f
│   └── 08e740fa27a1687a232ed74c0e16971baf99ad
├── 8a
│   └── 122028a529e462811a84575c12014558a65dac
├── 99
│   └── 048c3f7870798e427f005ccd8083dd14d91b48
├── info
└── pack

8 directories, 5 files

# show current index (state of staging)
$ git ls-files --stage
100644 99048c3f7870798e427f005ccd8083dd14d91b48 0       README.md

# commit README
$ git commit -m "docs: add contents about internal of Git and references"
[main 72c8662] docs: add contents about internal of Git and references
 1 file changed, 94 insertions(+)

# show .git/objects (changed!)
.git
├── COMMIT_EDITMSG
├── HEAD
├── config
├── description
├── hooks
│   ...
├── index
├── info
│   └── exclude
├── logs  # <--- created
│   ├── HEAD
│   └── refs
│       └── heads
│           └── main
├── objects
│   ├── 09  # <--- created
│   │   └── dba21c31dc9a6b94fe2825a41e45a5e25c4451
│   ├── 29
│   │   └── bfa74fe2e6b2bcd512b81e1e71a8dcb1ef0f0d
│   ├── 5f
│   │   └── 79def7d5c15303a8a565fe28fb9d66dceaec98
│   ├── 72  # <--- created
│   │   └── c8662ebc4cd6fc01f42560a479b3ff2e4ef05d
│   ├── 7f
│   │   └── 08e740fa27a1687a232ed74c0e16971baf99ad
│   ├── 8a
│   │   └── 122028a529e462811a84575c12014558a65dac
│   ├── 99
│   │   └── 048c3f7870798e427f005ccd8083dd14d91b48
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   └── main
    ├── remotes
    │   └── origin
    │       └── main
    └── tags

23 directories, 31 files
```

#### `objects`

```sh
$ tree .git/objects
.git/objects
├── 29
│   └── bfa74fe2e6b2bcd512b81e1e71a8dcb1ef0f0d
├── 5f
│   └── 79def7d5c15303a8a565fe28fb9d66dceaec98
├── 7f
│   └── 08e740fa27a1687a232ed74c0e16971baf99ad
├── 8a
│   └── 122028a529e462811a84575c12014558a65dac
├── 99
│   └── 048c3f7870798e427f005ccd8083dd14d91b48
├── info
└── pack

8 directories, 5 files
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

$ git log
commit 72c8662ebc4cd6fc01f42560a479b3ff2e4ef05d (HEAD -> main)
Author: nukopy <nukopy@gmail.com>
Date:   Wed May 31 04:35:09 2023 +0900

    docs: add contents about internal of Git and references

commit 29bfa74fe2e6b2bcd512b81e1e71a8dcb1ef0f0d (origin/main)
Author: nukopy <nukopy@gmail.com>
Date:   Wed May 31 03:00:35 2023 +0900

    Initial commit

$ cat .git/refs/heads/main
72c8662ebc4cd6fc01f42560a479b3ff2e4ef05d

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

-->

### commands of cargo

```sh
# rustc
rustc main.rs
./main
rustc main.rs -o bin/rit
./bin/rit

# cargo
cargo new [project name]
cargo init
cargo build           # build for development (no optimization, fast build time, slow runtime)
cargo build --release # build for release (optimization, slow build time, fast runtime)
cargo run
cargo run --release
cargo check
cargo update
cargo test
```

## References

- [(Zenn, 2020/11) Rust でつくる Git 入門](https://zenn.dev/uzimaru0000/books/impl-git-in-rust)
- [github.com/uzimaru0000/clumsy](https://github.com/uzimaru0000/clumsy)
  - reference implementation of Git in Rust
- official documentation of Git
  - [10.1 Git の内側 - 配管（Plumbing）と磁器（Porcelain）](https://git-scm.com/book/ja/v2/Git%E3%81%AE%E5%86%85%E5%81%B4-%E9%85%8D%E7%AE%A1%EF%BC%88Plumbing%EF%BC%89%E3%81%A8%E7%A3%81%E5%99%A8%EF%BC%88Porcelain%EF%BC%89)
  - [10.2 Git の内側 - Git オブジェクト](https://git-scm.com/book/ja/v2/Git%E3%81%AE%E5%86%85%E5%81%B4-Git%E3%82%AA%E3%83%96%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88)
- [(2015/09) mercari engineering - Git のつくりかた](https://engineering.mercari.com/blog/entry/2015-09-14-175300/)
- [(Slideshare, 2015/08) Git のつくりかた YAPC::Asia 2015 @DQNEO](https://www.slideshare.net/DQNEO/git-yapcasia-2015-dqneo)
- [(Zenn, 2021/08)Git のインデックスの中身](https://zenn.dev/kaityo256/articles/inside_the_index)
- [github.com/git/git](https://github.com/git/git)
  - Git Source Code Mirror
- [github.com/DQNEO/minigit](https://github.com/DQNEO/minigit)
  - Implementation of Git in C
- [github.com/TaKO8Ki/gobang](https://github.com/TaKO8Ki/gobang)
  - A cross-platform TUI database management tool written in Rust
- [Rust エディションガイド](https://doc.rust-jp.rs/edition-guide/editions/index.html)
