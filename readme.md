# Blog

An attempt at creating a blog using markdown.

I'm trying out hugo.

## Write

`hugo new content posts/post-name.md`.

## Run

`hugo server -D` will include drafts.

## Build

`hugo`

Add `--cleanDestinationDir` to remove other things.

## Deploy

Site is published automatically on push to main. It's hosted on github pages.

## Notes

Followed the help in these pages to set up github actions to push to github
pages, with a custom domain and https:
https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site

Set up prettier, but don't wanna install it again, so just using whatever is in
vscode.

See `.github/workflows/hugo.yml` for the script that publishes on commit to
main. I found that somewhere, but I've changed it a lot, since I wanted to use
docker to build.
