# Hugo template for Netlify CMS + NetlifyCMS Custom Widget (@ncwidgets)

Folks mainly use [NetlifyCMS Custom Widgets](https://github.com/d4rekanguok/netlify-cms-widgets) for its file relation widget. This is an example of how to use it with Hugo.

## Setup

1. install dependencies

```bash
npm install
```

2. run webpack, hugo & the development CMS backend all at once

```bash
npm start
```

3. Navigate to `localhost:3000/admin` & click on any post. The categories field is a file-relation widget.

## Apply this to your Hugo site

1. Set up a JS build pipeline

> Unfortunately, having a JS build pipeline -- be it Webpack, Parcel or Rollup -- is the best way to use custom widgets with netlifyCMS. It allows for much lighter bundle & access to the larger React ecosystem. For example, without a bundler, `file-relation` can't access [`react-select`](https://react-select.com/home), which allows multiple select, suggestion dropdown, etc. 

The template this repo is forked from use Webpack so that's what I use, but for simple no config setup, I personally prefer [Parcel](https://parceljs.org/).

2. Install the custom widgets you need

```sh
npm install @ncwidgets/file-relation @ncwidgets/id
```

3. Add it to NetlifyCMS

```js
// src/js/cms.js or wherever you set this file up
import CMS from "netlify-cms-app";

import {Widget as RelationWidget} from "@ncwidgets/file-relation";
import {Widget as IdWidget} from "@ncwidgets/id";

// use 'ncw-file-relation' in config.yml
CMS.registerWidget(RelationWidget);

// use 'ncw-id' in config.yml
CMS.registerWidget(IdWidget);

/* If you prefer to use your own widget name in config.yml, do this
 * CMS.registerWidget({
 *   name: 'my-custom-name',
 *   ...RelationWidget, 
 * })
 */

CMS.init();
```

4. Use these widgets in your config.yml

`file-relation` looks for a list in a file collection. See the [readme here](https://github.com/d4rekanguok/netlify-cms-widgets/blob/master/packages/widget-file-relation/readme.md) for a detail instruction of how to use it.

In this example repo, I add a `Setting` collection, which has a `categories` file (store at `site/content/setting.json`) with a `categories` list which houses all the options for `file-relation`.

The config file is stored at `site/static/config.yml`.

```yml
# setup file-relation
collections:
  - name: "post"
    label: "Post"
    folder: "site/content/post"
    create: true
    fields:
      - label: "Categories"
        name: categories
        widget: ncw-file-relation
        collection: setting
        file: categories
        target_field: categories
        id_field: id
        display_fields: name
        multiple: true
        hint: This field use @ncwidgets/file-relation

# setup a file collection with a list
  - name: "setting"
    label: Setting
    files:
      - file: "site/content/setting.json"
        label: Categories
        name: categories
        fields:
          - label: Categories
            name: categories
            widget: list
            allow_add: true
            fields:
              - label: Name
                name: name
                widget: string
              - label: ID
                name: id
                widget: ncw-id
```

> `@ncwidgets/id` will generate a unique ID for each new item in `categories` list. If you already have a unique id for each item, you don't need to use this extra widget. It's not a requirement to use `@ncwidgets/id` with `@ncwidgets/file-relation`.

# Hugo template for Netlify CMS with Netlify Identity

This is a small business template built with [Victor Hugo](https://github.com/netlify/victor-hugo) and [Netlify CMS](https://github.com/netlify/netlify-cms), designed and developed by [Darin Dimitroff](http://www.darindimitroff.com/), [spacefarm.digital](https://www.spacefarm.digital).

## Getting started

Use our deploy button to get your own copy of the repository. 

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/netlify-templates/one-click-hugo-cms&stack=cms)

This will setup everything needed for running the CMS:

* A new repository in your GitHub account with the code
* Full Continuous Deployment to Netlify's global CDN network
* Control users and access with Netlify Identity
* Manage content with Netlify CMS

Once the initial build finishes, you can invite yourself as a user. Go to the Identity tab in your new site, click "Invite" and send yourself an invite.

Now you're all set, and you can start editing content!

## Local Development

Clone this repository, and run `yarn` or `npm install` from the new folder to install all required dependencies.

Then start the development server with `yarn start` or `npm start`.

## Layouts

The template is based on small, content-agnostic partials that can be mixed and matched. The pre-built pages showcase just a few of the possible combinations. Refer to the `site/layouts/partials` folder for all available partials.

Use Hugo’s `dict` functionality to feed content into partials and avoid repeating yourself and creating discrepancies.

## CSS

The template uses a custom fork of Tachyons and PostCSS with cssnext and cssnano. To customize the template for your brand, refer to `src/css/imports/_variables.css` where most of the important global variables like colors and spacing are stored.

## SVG

All SVG icons stored in `site/static/img/icons` are automatically optimized with SVGO (gulp-svgmin) and concatenated into a single SVG sprite stored as a a partial called `svg.html`. Make sure you use consistent icons in terms of viewport and art direction for optimal results. Refer to an SVG via the `<use>` tag like so:

```
<svg width="16px" height="16px" class="db">
  <use xlink:href="#SVG-ID"></use>
</svg>
```
