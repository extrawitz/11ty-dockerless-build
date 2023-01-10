## Build Eleventy (11ty)

This action fetches all dependencies via **npm install** which are specified in your **.eleventy.js** and builds then the website.

No docker is used. No root-permissions needed on your self-hosted runner

### Features
- this action runs flawless without root-permissions as docker is not used.
- you can use most common plugins, i.e. **@11ty/eleventy-img** which ist not possible with docker-gh-actions.
- You can pass custom args to the eleventy-command

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
