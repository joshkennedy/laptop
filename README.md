# Laptop

Laptop is a script to set up an macOS laptop for web and mobile development.

- It can be run multiple times on the same machine safely.
- It installs, upgrades, or skips packages
- based on what is already installed on the machine.

## Requirements

We support:

- macOS Mavericks (10.9)
- macOS Yosemite (10.10)
- macOS El Capitan (10.11)
- macOS Sierra (10.12)

Older versions may work but aren't regularly tested.

## Install

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/joshkennedy/laptop/master/mac
```

Review the script (avoid running scripts you haven't read!):

```sh
less mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

Optionally, review the log:

```sh
less ~/laptop.log
```

## What it sets up

#### macOS tools:

* [Homebrew] for managing operating system libraries.
* [Slack] for internal team communication
* [iTerm2] for a better terminal experience
* [Atom], [Sublime Text], and [Visual Studio] for coding magicks

[Homebrew]: http://brew.sh/
[Slack]: http://slackhq.com/
[iTerm2]: https://www.iterm2.com/
[Atom]: https://atom.io
[Sublime Text]: https://www.sublimetext.com/
[Visual Studio]: https://www.visualstudio.com/


#### Unix tools:

* [Git] for version control
* [The Silver Searcher] for finding things in files
* [Watchman] for watching for filesystem events
* [Zsh] as your shell

[Git]: https://git-scm.com/
[The Silver Searcher]: https://github.com/ggreer/the_silver_searcher
[Watchman]: https://facebook.github.io/watchman/
[Zsh]: http://www.zsh.org/

#### Image tools:

* [ImageMagick] for cropping and resizing images

[ImageMagick]: http://www.imagemagick.org/

#### Testing tools:

* [Qt 5] for headless JavaScript testing via [Capybara Webkit]

[Qt 5]: http://qt-project.org/
[Capybara Webkit]: https://github.com/thoughtbot/capybara-webkit

#### Programming languages, package managers, and configuration:

* [Bundler] for managing Ruby libraries
* [Node.js] and [NPM], for running apps and installing JavaScript packages
* [Ruby] stable for writing general-purpose code
* [Yarn] for managing JavaScript packages

[Bundler]: http://bundler.io/
[Node.js]: http://nodejs.org/
[NPM]: https://www.npmjs.org/
[Ruby]: https://www.ruby-lang.org/en/
[Yarn]: https://yarnpkg.com/en/

#### Databases:

* [Postgres] for storing relational data

[Postgres]: http://www.postgresql.org/

It should take less than 15 minutes to install (depending on your machine).

Customize in `~/.laptop.local`
------------------------------

Your `~/.laptop.local` is run at the end of the Laptop script. Put your customizations there.

For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

Write your customizations such that they can be run safely more than once. See the `mac` script for examples.

Laptop functions such as `fancy_echo` and `gem_install_or_update` can be used in your `~/.laptop.local`.

See the [wiki](https://github.com/thoughtbot/laptop/wiki) for more customization examples.

## License

Laptop is © 2011-2017 thoughtbot, inc.
It is free software,
and may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: LICENSE
