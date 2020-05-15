# openbylaws.org.za microsites

[![Build Status](https://travis-ci.com/laws-africa/openbylaws.org.za.svg?branch=master)](http://travis-ci.com/laws-africa/openbylaws.org.za)

This repo is for the openbylaws.org.za microsites, based on Jekyll. It builds the standalone microsites for some
municipalities.

It is a fork of the [openbylaws.org.za repo](https://github.com/laws-africa/openbylaws.org.za) with minor customisations
to make it single-municipality specific.

There are three major steps to building this website:

1. `bin/place.py` is a Python script to update the configuration and content to be for a particular municipality.
2. `jekyll build` builds the website as a regular Jekyll website.

In production, these steps are performed by Travis-CI, and the resulting site is force pushed to the `gh-pages` branch as a static website.

## But where is the content?

This repo looks a bit empty, there are no places and by-laws. This saves us from having a repo tracking every change to every by-law. Instead, this repo is fleshed out by Travis-CI during the build phase.

## Local development

1. Clone this repo
2. Tell git to fetch only the master branch, and not the `gh-pages` branch, which is built automatically and is very big. Edit `.git/config` and change the `fetch` line of the `[remote "origin"]` section to look like this: `fetch = +refs/heads/master:refs/remotes/origin/master`
2. Install Python dependencies: `pip install -r requirements.txt`
3. Install Jekyll: `bundle install`
4. Get your Laws.Africa API token from [edit.laws.africa/accounts/profile/api/](https://edit.laws.africa/accounts/profile/api/)
5. `export INDIGO_API_AUTH_TOKEN=your-token`
6. Configure the site and pull in by-law content for just one municipality: `bin/place.py --quick za-wc033`
7. Build the website: `bundle exec jekyll server --watch --incremental`

## Adding a new municipality

1. Add the municipality name, code and by-laws website URL to `_data/places.json`
2. Add the municipality logo and placard images to `img/municipalities/`, using the naming format `<code>-logo.png` and `<code>-placard.jpg`. Note that the two files have different extensions.
3. Push to master
4. Create a fork of [openbylaws-wc033](https://github.com/laws-africa/openbylaws-wc033), which will host the GitHub pages for the site and trigger a build.
5. In that new repo, update `.travis.yml` to set the PLACE variable to the appropriate place code, and set the `fqdn` variable to the FQDN of the by-laws microsite.
6. Still in that repo, replace the two secrets in `.travis.yml` with secrets for `INDIGO_API_AUTH_TOKEN` and `GITHUB_TOKEN`.
7. Push that repo to GitHub
8. Setup DNS for the new microsite to point at GitHub pages.
