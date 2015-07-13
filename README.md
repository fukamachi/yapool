# Yapool

**Yapool** is a commond-line tool for executing shell commands via SSH for application deployment or systems administration tasks. It is inspired by Ruby's [Capistrano](http://capistranorb.com) and Python's [Fabric](http://fabfile.org).

## Usage

```
$ yap deploy
$ YAP_ENV=production yap deploy
```

## Installation

As Yapool depends on [Roswell](https://github.com/snmsts/roswell), you have to install it before. Roswell is a Common Lisp implementation manager and also provides command-line interface for Common Lisp.

Yapool is not ready for installing via Quicklisp yet. Download it from [GitHub](https://github.com/fukamachi/yapool) and put it into a directory ASDF can find (like `~/common-lisp`).

```
$ cd ~/common-lisp
$ git clone https://github.com/fukamachi/yapool
$ ros -l yapool/yapool.asd install yapool
```

After doing the above instruction, `yap` command is available and can see it's help.

```
$ yap --help
```

If your shell says that `yap` command is not found, check `~/.roswell/bin` is in your shell's PATH.

## Quickstart

A project that intends to use Yapool must have `yapfile.lisp` and `yap/` directory at the project root.

`yapfile.lisp` is a configuration file for defining environmental variables like server names. Yapool adopts [Envy](https://github.com/fukamachi/envy) to define them.

The file will be loaded in `:yapool-user` (= `:yap-user`) package which imports symbols in `:cl` and `:envy` package.

```common-lisp
(in-package :yap-user)

(defconfig :common
  '(:user "fukamachi"
    :directory #P"/home/quickdocs/quickdocs/"))

(defconfig |production|
  '(:hosts ("10.10.2.71" "10.10.2.72")))

(defconfig |staging|
  '(:hosts ("10.0.12.04")))
```

See [documentation of Envy](https://github.com/fukamachi/envy) for getting how to use it.

`yap/` directory is a location for Yapool's subcommands of the project. Those scripts are Roswell scripts which will be executed by `yap` command. For instance, `yap deploy` will invoke `yap/deploy`.

## Useful Packages

These packages are parts of Yapool and can be used in subcommand scripts.

### yap.shell

* `run`: Execute a shell command in a remote server.
* `sudo`: Execute a shell command like `run` except this requires sudo priviledge.
* `with-directory`: Change the current directory in a remote server during executing the body.

## See Also

* [Roswell](https://github.com/snmsts/roswell)
* [trivial-ssh](https://github.com/eudoxia0/trivial-ssh)
* [Envy](https://github.com/fukamachi/envy)

## Author

* Eitaro Fukamachi (e.arrows@gmail.com)

## Copyright

Copyright (c) 2015 Eitaro Fukamachi (e.arrows@gmail.com)

## License

Licensed under the BSD 3-Clause License.
