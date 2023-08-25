# How to setup blog locally

1. Install Rbenv (ruby version manager):
    - macOS: `brew install rbenv ruby-build`
    - Linux (Debian, Ubuntu and their derivatives): `sudo apt install rbenv`
2. Load rbenv to shell: `rbenv init` (follow instructions after run the command)
3. Install ruby 3.2.2: `rbenv install 3.2.2`
4. Clone this repo
5. Cd to repo folder and then use ruby 3.2.2: `rbenv local 3.2.2`
6. Install bundle: `gem install bundle`
7. Install required packages (already defined in Gemfile): `bundle install`.
8. Start local: `bundle exec jekyll serve`
