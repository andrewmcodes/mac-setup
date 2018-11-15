# Mac-setup

Mac-setup is a script to set up an macOS laptop for web and mobile development.

## Requirements

We support:

- macOS High Sierra (10.13)
- macOS Mojave (10.14)

## Install

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/andrewmcodes/mac-setup/master/setup
```

Review the script (avoid running scripts you haven't read!):

```sh
less setup
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/setup.log
```

Optionally, review the log:

```sh
less ~/setup.log
```

## Debugging

Your last mac-setup run will be saved to `~/setup.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/andrewmcodes/mac-setup/new) for us.
Or, attach the whole log file as an attachment.

## What it sets up

macOS tools:

- [Homebrew] for managing operating system libraries.

[homebrew]: http://brew.sh/

Unix tools:

- [Exuberant Ctags] for indexing files for vim tab completion
- [Git] for version control
- [OpenSSL] for Transport Layer Security (TLS)
- [RCM] for managing company and personal dotfiles
- [The Silver Searcher] for finding things in files
- [Tmux] for saving project state and switching between projects
- [Watchman] for watching for filesystem events
- [Zsh] as your shell

[exuberant ctags]: http://ctags.sourceforge.net/
[git]: https://git-scm.com/
[openssl]: https://www.openssl.org/
[rcm]: https://github.com/thoughtbot/rcm
[the silver searcher]: https://github.com/ggreer/the_silver_searcher
[tmux]: http://tmux.github.io/
[watchman]: https://facebook.github.io/watchman/
[zsh]: http://www.zsh.org/

Heroku tools:

- [Heroku CLI] and [Parity] for interacting with the Heroku API

[heroku cli]: https://devcenter.heroku.com/articles/heroku-cli
[parity]: https://github.com/thoughtbot/parity

GitHub tools:

- [Hub] for interacting with the GitHub API

[hub]: http://hub.github.com/

Image tools:

- [ImageMagick] for cropping and resizing images

Testing tools:

- [Qt 5] for headless JavaScript testing via [Capybara Webkit]

[qt 5]: http://qt-project.org/
[capybara webkit]: https://github.com/thoughtbot/capybara-webkit

Programming languages, package managers, and configuration:

- [ASDF] for managing programming language versions
- [Bundler] for managing Ruby libraries
- [Node.js] and [NPM], for running apps and installing JavaScript packages
- [Ruby] stable for writing general-purpose code
- [Yarn] for managing JavaScript packages

[bundler]: http://bundler.io/
[imagemagick]: http://www.imagemagick.org/
[node.js]: http://nodejs.org/
[npm]: https://www.npmjs.org/
[asdf]: https://github.com/asdf-vm/asdf
[ruby]: https://www.ruby-lang.org/en/
[yarn]: https://yarnpkg.com/en/

Databases:

- [Postgres] for storing relational data
- [Redis] for storing key-value data

[postgres]: http://www.postgresql.org/
[redis]: http://redis.io/

It should take less than 15 minutes to install (depends on your machine).

## Customize in `~/.setup.local`

Your `~/.setup.local` is run at the end of the mac-setup script.
Put your customizations there.
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

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

## Contributing

Edit the `mac` file.
Document in the `README.md` file.
Follow shell style guidelines by using [ShellCheck] and [Syntastic].

```sh
brew install shellcheck
```

[shellcheck]: http://www.shellcheck.net/about.html
[syntastic]: https://github.com/scrooloose/syntastic
