# ohmyzsh-helper

Everything in this README is meant to be added to the end of your `~/.zshrc` file.

## Switch PHP versions

Abstract:
```shell
function phpMAJORMINOR() {
    sudo update-alternatives --set php /usr/bin/phpMAJOR.MINOR
    sudo update-alternatives --set php-config /usr/bin/php-configMAJOR.MINOR
    sudo update-alternatives --set phpize /usr/bin/phpizeMAJOR.MINOR
}
```

Example:
```shell
function php81() {
    sudo update-alternatives --set php /usr/bin/php8.1
    sudo update-alternatives --set php-config /usr/bin/php-config8.1
    sudo update-alternatives --set phpize /usr/bin/phpize8.1
}
```

## Homestead helper

For use with [Laravels Homestead](https://laravel.com/docs/9.x/homestead)

```shell
function homestead() {
    ( cd ~/Homestead && vagrant $* )
}

function homestead_config() {
    ( cd ~/Homestead && code Homestead.yaml )
}
```

## Switch composer versions

For example when you need to work on old projects which are not compatible to Composer 2.

```shell
function composer1() {
    sudo composer self-update --1
}

function composer2() {
    sudo composer self-update --2
}
```

## Binary dirs

I'd recommend always having a `/bin` directory in your home folder:
```shell
export PATH="$HOME/bin:$PATH"
```

Also, if you have composer packages installed globally:
```shell
export PATH="$(composer config -g home)/vendor/bin:$PATH"
```

## Drush helpers

```shell
# Include Drush bash customizations.
if [ -f "/home/thomas/.drush/drush.bashrc" ] ; then
  source /home/thomas/.drush/drush.bashrc
fi

# Include Drush prompt customizations.
if [ -f "/home/thomas/.drush/drush.prompt.sh" ] ; then
  source /home/thomas/.drush/drush.prompt.sh
fi
```

## PHPCS function for use with Drupal

```shell
function phpcs() {
  phpcs --colors --standard=Drupal --extensions=php,module,inc,install,test,profile,theme,js,css,info,txt,md,yml $*;
  phpcs --colors --standard=DrupalPractice --extensions=php,module,inc,install,text,profile,theme,js,css,info,txt,mkd,yml $*
}
```

## Aliases

```shell
alias ll="ls -lash"
alias .="ls -lash"
alias ..="cd .."
alias ...="cd"
```

## Git profiles

Don't forget to change profile-n-x to real values.

```shell
function profile-1-function() {
    git config --global user.name "profile-1-name"
    git config --global user.email "profile-1-email"
    echo "Swapped git profile to profile-1"
}

function profile-2-function() {
    git config --global user.name "profile-2-name"
    git config --global user.email "profile-2-email"
    echo "Swapped git profile to profile-2"
}

preexec () {
    if [[ -d ".git" ]]; then
        if [[ $(git remote get-url origin | grep profile-1-url) ]]; then
            if [[ $(git config --global user.email) != "profile-1-email" ]]; then
                profile-1-function
            fi
        else
            if [[ $(git config --global user.email) != "profile-2-email" ]]; then
                profile-2-function
            fi
        fi
    fi
}
```