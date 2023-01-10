## Build Eleventy (11ty)

![](IdthKOzqFA-350.avif)

This action fetches all dependencies via **npm install** which are specified in your **.eleventy.js** and builds then the website.

No docker is used. No root-permissions needed on your self-hosted runner

### Features
- this action runs flawless without root-permissions as docker is not used.
- you can use most common plugins, i.e. **@11ty/eleventy-img** which ist not possible with docker-gh-actions.
- You can pass custom args to the eleventy-command

### Pre-Requisites
- your site has to run smoothly on your local machine.
- You need a proper ".eleventy.js" file https://www.11ty.dev/docs/config/
- You will need proper passthrough config i.e. for your assets-folder https://www.11ty.dev/docs/copy/
- You need to have a **valid local package.json** in your repo, so that **npm** / **npx** works clean.

### Example-Workflow

```
name: 11ty build
on: [push]
jobs:
  build_deploy:
   runs-on: self-hosted
   steps:
   - name: Checkout last commit of repository
	 uses: actions/checkout@v3
   - name: Install 11ty and build website
	 uses: extrawitz/11ty-dockerless-build@v1
   - name: Deploy
	 uses: peaceiris/actions-gh-pages@v3
	 with:
	  publish_dir: public
	  publish_branch: website
	  github_token: ${{ secrets.PSA }}
```

#### Use your custom args when building website with "eleventy"

To use your custom arguments, specify them in your workflow like in below example:

```
name: 11ty build
on: [push]
jobs:
  build_deploy:
   runs-on: self-hosted
   steps:
   - name: Checkout last commit of repository
	 uses: actions/checkout@v3
   - name: Install 11ty and build website
	 uses: extrawitz/11ty-dockerless-build@v1
	 with:
	  args: --formats=md,html,ejs
   - name: Deploy
	 uses: peaceiris/actions-gh-pages@v3
	 with:
	  publish_dir: public
	  publish_branch: website
	  github_token: ${{ secrets.PSA }}
```

See more details about this in eleventy-doc
https://www.11ty.dev/docs/usage/

### Pro-Tipp to keep your main-branch clean when working with Eleventy

You can exclude your public-folder from excludes wih adding a entry for the public folder into your .git/info/exclude file

#### Example
```
# git ls-files --others --exclude-from=.git/info/exclude
# Lines that start with '#' are comments.
# For a project mostly in C, the following would be a good set of
# exclude patterns (uncomment them if you want to use them):
# *.[oa]
# *~

public/*
```

More Infos on that here https://git-scm.com/docs/gitignore#_description

PS @ Microsoft - branch specific .gitignores would be alltough nice :-)
