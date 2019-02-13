```sh
$ git clone https://github.com/xe1gyq/linux.git
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ git checkout -b v4.14 v4.14
Checking out files: 100% (42539/42539), done.
Switched to a new branch 'v4.14'
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ make oldconfig
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ make -j5
```
