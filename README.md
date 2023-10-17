# Install and run server locally

Note for macOS users: install Homebrew first.
Note for Windows users: install Chocolatey first. Sometimes Dart Sass will show error when install, don't panic, just searching around Stackoverflow 🥲.

1. Install golang at https://go.dev/doc/install
2. Install Dart Sass (follow instructions here for each OS: https://gohugo.io/hugo-pipes/transpile-sass-to-css/#dart-sass)
3. Install hugo (choose your installation method based on your OS here: https://gohugo.io/installation/)
4. Pull this repo
5. Run hugo server -D

# Write posts

1. Checkout another branch before do anything because master branch is locked.
2. All post goes under folder `content/posts` with markdown format. You can wrap all your related posts under sub folder like this `content/posts/java-series`.
3. If your name is not listed inside list authors in `hugo.yaml` under `params.authors` section, feel free to add your name or nickname or whatever you want to show as author. Only when you added your name into this list authors, you can add your name into posts metadata.
4. Do not put video file inside this repo. If you want to embed a video, upload to YouTube first, then embed the link only.
5. Images are allowed but try to minimize the size (you can use free only tools for resize/crop). Each post has only 5MB of size only including text, assets like images. For even better, you can upload images to sources like Imgur, Google Photos, etc. and get image link and put into your post. If you use the images directly from codebase, put it under the same folder of your posts and ref it with `![An image here](./image.png)`.
6. Make sure your post is ready (run and view as expected on local first) before create pull request to master. Everything goes into master will be deployed automatically and visible on UI.
7. You can refer to PaperMod documentation for more details about the markdown format they use for rich content, math typeset, emoji.
8. When create a post, you can follow the metadata format inside `template/template.md`.
9. The master header is `h2` which is ## in markdown. Don't use `h1` or # because it is already used for the post title.
10. The image scaling is currently handled by the Papermod theme and it's kinda suck so in the mean time just do the resizing image manually and I will fix the scaling in the theme later. 😢

# Deploy posts

1. Create a pull request from your branch to master.
2. Please ask for review from other team members.
3. Double check everything before deploy like syntax, indentation, spelling, grammar, etc.
4. Once finish, ask Ming to merge PR, the build is automatically and take about a few minutes to finish.

# Some warnings

* If you want to change anything in blog configuration, please ask Ming first to get approval, then checkout branch `Config`, merge `master` into `Config` if needed and do the modification on `Config`. After finish, create a pull request, ask for review from Ming and merge into `master`.
* Please do not change anything inside `themes/PaperMod` and `layouts/partials` folder. It can break the UI and I have to do revert it.
