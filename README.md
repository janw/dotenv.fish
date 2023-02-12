# dotenv.fish

Automatically load environment variables from a .env.fish file when you cd into a directory.

A fish port of the popular [OhMyZsh dotenv plugin](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/dotenv).

## Setup

Install with [Fisher](https://github.com/jorgebucaran/fisher):

```fish
fisher install janw/dotenv.fish
```

## Usage

Create `.env.fish` file inside your project root directory and put your environment variables there.

For example:

```fish
set -gx AWS_S3_TOKEN d84a83539134f28f412c652b09f9f98eff96c9a
set -gx SECRET_KEY 7c6c72d959416d5aa368a409362ec6e2ac90d7f
set -gx MONGO_URI mongodb://127.0.0.1:27017
set -gx PORT 3001
```

Since dotenv.fish simply sources the file, the contents are not limited to exporting environment variables. You may also create shell aliases or functions, and source external fish components that are relevant to the project in the current directory, etc. With this great power comes great responsibility, too, tough. Be mindful of what you place in your `.env.fish` files. The plugin only ensures basic syntax sanity, and sources the file. Nothing less, nothing more. It doesn't do any checks. It is designed to be simple and fast option.

## Settings

The behavior of dotenv.fish can be altered using the following config variables, to be placed in your `~/.config/fish/config.fish`:

### FISH_DOTENV_FILE

By default dotenv.fish looks for the `.env.fish` file in the current directory. If you want it to source another file you can modify the `FISH_DOTENV_FILE` variable. For example, this will make the plugin look for files named `.dotenv.fish` and load them:

```fish
set FISH_DOTENV_FILE=.dotenv.fish
```

### FISH_DOTENV_ALLOWLIST and FISH_DOTENV_BLOCKLIST

The default behavior of the plugin is to ask you whether to source an env file, with the options being **Y**es, **N**o, **A**lways and N**e**ver:

* If you choose Always (typing letter `a`), the directory of the env file will be added to an allowlist, and the plugin will not you for confirmation again for this directory, automatically sourcing the file.
* If you choose Never (typing letter `e`), it will be added to the blocklist, and the file will not be sourced in subsequent visits to the directory.
* Yes (`y`) and No (`n`) behave in just the same way but only apply to the current moment. When you cd into the directory again, you'll also be asked again.

By default the allowlist and blocklist are placed in your fish config directory under `.dotenv-allowed.list` and `dotenv-blocked.list` respectively. If you want to change that location, change the `$FISH_DOTENV_ALLOWLIST` and `$FISH_DOTENV_BLOCKLIST` variables like so if you wanted to place it in your XDG cache home:

```fish
set -g FISH_DOTENV_ALLOWLIST "$XDG_CACHE_HOME/.dotenv-allowed.list"
set -g FISH_DOTENV_BLOCKLIST "$XDG_CACHE_HOME/.dotenv-blocked.list"
```

Both files are just plain-text lists of paths, separated by a newline characters. If you change your decision about a certain directory, you can simply edit the respective file and remove the directory from it. Pleaas note that if a directory exists in both the allowlist and blocklist, the blocklist takes preference: the env file will never be sourced.

## Version Control

It's recommended to add `.env.fish` file to your project's `.gitignore` file, because it usually contains sensitive information such as credentials, secret keys, passwords etc. that you don't want to commit to version control. The env file is supposed to be local-only.
