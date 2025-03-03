# Contributing to Cypress Documentation

Thanks for taking the time to contribute! :smile:

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Writing Documentation](#writing-documentation)
  - [Using Vue Components](#using-vue-components)
    - [Alerts](#alerts)
    - [Images](#images)
    - [Videos](#videos)
    - [Icons](#icons)
  - [Adding Examples](#adding-examples)
  - [Adding Plugins](#adding-plugins)
  - [Adding Pages](#adding-pages)
  - [Writing the Changelog](#writing-the-changelog)
- [Committing Code](#committing-code)
  - [Linting](#linting)
  - [Pull Requests](#pull-requests)
  - [Contributor License Agreement](#contributor-license-agreement)
- [Deployment](#deployment)

## Code of Conduct

All contributors are expected to abide by our [Code of Conduct](https://github.com/cypress-io/cypress/wiki/code-of-conduct).

## Writing Documentation

The documentation uses [Nuxt.js](https://nuxtjs.org/) to generate a static website. Refer to the [Nuxt.js](https://nuxtjs.org/docs/2.x/get-started/installation) documentation for specifics about the Nuxt.js framework.

**Fork this repository**

Using GitHub, [create a copy](https://guides.github.com/activities/forking/) (a fork) of this repository under your personal account.

**Clone your forked repository**

```shell
git clone git@github.com:<your username>/cypress-documentation.git
cd cypress-documentation
```

### Using Vue Components

This project uses [`@nuxt/content`](https://content.nuxtjs.org/) which enables you to write Vue components within markdown. Any component files placed within the `/components/global` directory will be available for use within the markdown files. There are a [few limitations](https://content.nuxtjs.org/writing#vue-components) with using Vue components in markdown.

#### Alerts

Use [`<Alert>`](/components/global/Alert.vue) to grab the reader's attention with a blurb. You can change the look of the `<Alert>` by setting the `type` prop to `info`, `tip`, `warning`, or `danger`.

```jsx
<Alert type="info">This is an important message.</Alert>
```

### Images

If you are starting a new page and want to add images, add a new folder to [`assets/img`](/assets/img). For example when adding a new "Code Coverage" page to `guides/tooling`, I have created new folder `assets/img/guides/tooling` and copied an image there called `coverage-object.png`. Within the markdown, I can include the image using the [`<DocsImage />` component](/components/global/DocsImage.vue).

```jsx
<DocsImage
  src="/assets/img/guides/tooling/coverage-object.png"
  alt="code coverage object"
/>
```

### Videos

You can embed videos within the markdown with the [`<DocsVideo>`](/components/global/DocsVideo.vue) component. Currently, it supports local files, YouTube, and Vimeo embeds. Set the `src` prop to a relative path for a local video file or the embed link for YouTube or Vimeo videos. You should also set a `title` prop describing the video for accessibility reasons.

```jsx
<DocsVideo
  src="https://www.youtube.com/embed/dQw4w9WgXcQ"
  title="Cypress Tips and Tricks"
>
```

### Icons

[Font Awesome](https://fontawesome.com/) icons can be used within markdown. Set the `name` prop to the name of the Font Awesome icon. Make sure that the icon appears in the list of imported icons within the `nuxt.config.js` file under the `fontawesome` key.

```jsx
<Icon name="question-circle"></Icon>
```

### Adding Examples

To add a course, blog, talk, podcast, or screencast to our docs, submit a [pull request](#Pull-Requests) with your data added to the corresponding [courses.json](/content/_data/courses.json), [blogs.json](/content/_data/blogs.json), [talks.json](/content/_data/talks.json), [podcasts.json](/content/_data/podcasts.json) or [screencasts.json](/content/_data/screencasts.json) file.

Add an associated image with the example within the [`assets/img`](/assets/img) directory. Each image should have a resolution of **715×480**. Reference the image in the markdown document as follows:

```jsx
<DocsImage src="/img/examples/name-of-file.jpg" alt="alt text describing img" />
```

### Adding Plugins

To add a plugin, submit a [pull request](#Pull-Requests) with the corresponding data added to the [`plugins.json`](/content/_data/plugins.json) file. Your plugin should have a name, description, link to the plugin's code, as well as any keywords.

### Adding Pages

To add a page such as a new guide or API documentation:

- Add the new page to the relevant directory under [`content`](/content).
- Link to your new page in the [`sidebar.json`](/content/_data/sidebar.json).
- Add translations for the sidebar link for each supported language (for English, this is located in [`en.json`](/content/_data/en.json)).
- Build the documentation site locally so that you can visually inspect your new page and the links to it.
- **REQUIRED**: Commit the new file using git - we auto-generate the doc to display within each supported language, this auto-generation depends on the file existing in git.
- Submit a [pull request](#Pull-Requests) for your change.

> Note: If you need to change the overall layout of a page, you should create a Vue component within the `/pages` directory. The `/pages` directory contains the `_.vue` components responsible for generating the views for the routes `/guides`, `/api/`, `/plugins`, etc. If you wanted to create a guide page that has a different layout from the other guide pages, you would create a component file within `/guides` matching the route name that you want to use. For example, if I wanted to create a unique guide page without the sidebar about using the Dashboard, I would create a file called `/pages/guides/my-dashboard-guide.vue` and create a Vue component for the specific layout I want to create. The page will then be accessible at the route `/guides/my-dashboard-guide`.

### Deleting Pages

To delete a page:

- Delete the page from the relevant directory under [`content`](/content).
- Remove the link from the the [`sidebar.json`](/content/_data/sidebar.json).
- Remove the translations for the sidebar link for each supported language (for English, this is located in [`en.json`](/content/_data/en.json)).
- **REQUIRED**: Commit the change using git - we auto-remove the doc within each supported language, this auto-generation depends on the file being deleted in git, the build will not work until this is commited.
- Build the documentation site locally so that you can visually inspect and make sure it was properly deleted.

#### A Worked Example

Let's imagine that the Cypress team has just added a new command called `privateState` and you've picked up the task to document it.

API documentation for commands is in the [`content/api/commands`](/content/api/commands) directory.

1. Add a file called `privatestate.md` to that directory.
2. Write the document. Look to the existing documentation to see how to structure the content.
3. Open the [`content/_data/sidebar.json`](/content/_data/sidebar.json) file and add a link the new `privatestate` page. In this example we're adding a command so we'll add a link underneath the `api.commands` section.

```json
"api": {
  "commands": {
    "and": "and.html",
    "as": "as.html",
    "blur": "blur.html",
    // ...
    "privatestate": "privatestate.html"
  }
}
```

4. Open [`content/_data/en.json`](/content/_data/en.json) and add an English translation for that sidebar link's title. In this example we're adding a command so we'll add the following text:

```json
"sidebar": {
  "api": {
    "introduction": "Introduction"
    "api": "API"
    "commands": "Commands"
    // ...
    "privatestate": "privateState"
  }
}
```

5. Submit a [pull request](#Pull-Requests) for your change.

### Writing the Changelog

When adding to the Changelog, create a new file in [`content/_changelogs`](/content/_changelogs) named as the version number. Be sure to follow the category structure defined below (in this order). Each bullet point in the list should _always_ be associated to an issue on the [`cypress`](https://github.com/cypress-io/cypress) repo and link to that issue (except for Documentation changes).

#### Categories

- **Summary** - If it is a large release, you may write a summary explaining what the point of this release is (mostly used for breaking releases)
- **Breaking Changes** - The users current implementation of Cypress may break after updating.
- **Deprecations** - Features have been deprecated, but will not break after updating.
- **Features** - A new feature
- **Bugfixes** - A bug existed in Cypress and a PR fixed the issue
- **Misc** - Not a feature or bugfix, but work that was done. May be internal work that was done and associated with an issue
- **Documentation Changes** - our docs were updated based on behavior changes in release

## Committing Code

### Linting

Javascript code is linted with [ESLint](https://eslint.org/).

### Pull Requests

You should push your local changes to your forked GitHub repository and then open a pull request (PR) from your repo to the `cypress-io/docs` repo.

- The PR should be from your repository to the `develop` branch in `cypress-io/docs`
- When opening a PR for a specific issue already open, please use the `closes #issueNumber` syntax in the pull request description&mdash;for example, `closes #138`&mdash;so that the issue will be [automatically closed](https://help.github.com/articles/closing-issues-using-keywords/) when the PR is merged.
- Please check the "Allow edits from maintainers" checkbox when submitting your PR. This will make it easier for the maintainers to make minor adjustments, to help with tests or any other changes we may need.
  ![Allow edits from maintainers checkbox](https://user-images.githubusercontent.com/1271181/31393427-b3105d44-ada9-11e7-80f2-0dac51e3919e.png)

### Contributor License Agreement

We use a [`cla-assistant.io`](https://cla-assistant.io/) web hook to make sure every contributor assigns the rights of their contribution to Cypress.io. If you want to read the CLA agreement, its text is in this [gist](https://gist.github.com/bahmutov/cf22bc6c6b55219d0f9a76d04981f7ae).

After making a [pull request](#pull-requests), the CLA assistant will add a review comment. Click on the link and accept the CLA. That's it!

## Deployment

We will try to review and merge pull requests as fast as possible. After merging, we will deploy it to the staging environment, run E2E tests (using Cypress itself of course!), and then merge it into `master`, which will deploy it to the official [https://docs.cypress.io](https://docs.cypress.io) website. If you want to know our deploy process, read [DEPLOY.md](DEPLOY.md).

### Trigger workflow build

Due to CircleCI API limitations (even after 2 years), you cannot trigger a workflow build using the API. Thus if you need to build, test and deploy `develop` branch for example, your best bet is to create an empty GitHub commit in the [cypress-io/docs](https://github.com/cypress-io/docs) repository in the `develop` branch. We have added [make-empty-github-commit](https://github.com/bahmutov/make-empty-github-commit) as a dev dependency and set it as `make-empty-commit` NPM script in the [package.json](package.json).

To trigger production rebuild and redeploy, use personal GitHub token and run:

```shell
GITHUB_TOKEN=<your token> npm run make-empty-commit -- --message "trigger deploy" --branch master
```

As always, using [as-a](https://github.com/bahmutov/as-a) is recommended for storing and using sensitive environment variables.
