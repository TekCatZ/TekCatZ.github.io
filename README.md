# Install and run server locally

1. Install golang at https://go.dev/doc/install
2. Install Dart Sass (follow instructions here for each OS: https://gohugo.io/hugo-pipes/transpile-sass-to-css/#dart-sass)
3. Install hugo (choose your installation method based on your OS here: https://gohugo.io/installation/)
4. Pull this repo
5. Run hugo server -D

# Write post

1. Checkout another branch before do anything because master branch is locked
2. All post goes under folder `content/posts` with markdown format. You can wrap all your related posts under sub folder like this `content/posts/java-series`.
3. Make sure your post is ready (run and view as expected on local first) before create pull request to master. Everything goes into master will be deployed automatically and visible on UI.
4. You can refer to PaperMod documentation for more details about the markdown format they use for rich content, math typeset, emoji.
5. When create a post, you can follow the metadata format inside `template/template.md`.
