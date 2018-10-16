## Editing Home Page Content

To edit the content on the home page, namely the "Quick Blurb" section below the banner photo, update the `index.md` located in the root directory or [here](https://github.com/maniaclab/maniaclab.github.io/edit/master/index.md). As shown below, the you may edit the title and the subtitle.

```YAML
---
section:
    title: Quick Blurb
    subtitle: This section displays optional page content lorem ipsum
---

Ut enim ad minim veniam, quis nostrud e
```

In order to update the content, you may update the lorem-ipsum below the second ---

## Posts (NEWS)

To create a new news page, you can create a new markdown file inside the `_posts` directory [here](https://github.com/maniaclab/maniaclab.github.io/tree/master/_posts) by following the recommended file naming format:
```
YEAR-MONTH-DAY-title.MARKUP
```
Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. For example, the following are examples of valid post filenames:

```
2011-12-31-new-years-eve-is-awesome.md
2012-09-12-how-to-write-a-blog.md
```

Post requires front matter, everything in between the first and second --- are part of the YAML Front Matter, and everything after the second --- will be rendered with Markdown and show up as “Content”.
The following is a post file with different configurations you can add as example:

```yaml
---
layout: post
title: Headline Title Here
categories: [news, presentations, etc]
author: John Black
excerpt: Preview excerpt of article (could be first sentence of your article).
---

Continued Markdown Content here to be converted to HTML.
```

## Adding Projects

To create a new news page, you can create a new markdown file inside the `_projects` directory [here](https://github.com/maniaclab/maniaclab.github.io/tree/master/_projects). You may name the file anything you'd like (i.e. osg.md, slate.md, vc3.md), as long as it is an .md file.

Use the following example template:

```yaml
---
layout: post
title: SLATE
categories: [projects]
author: Jeremy Van
icon: /assets/projects/slate-logo.png
excerpt: Services Layered At The Edge
---

- Where "icon" refers to the location of your project's image.

- Remember to add a photo to the '/assets/projects/' directory, which you'll be able to reference here. The page will still render the markdown with or without the image.
```

- Once you've created this project's markdown, please also make a copy of this markdown file, and add it to the `/_docs` directory [here](https://github.com/maniaclab/maniaclab.github.io/tree/master/_docs), otherwise it will not come up in the search table of content (TOC) documentation. You may, quite literally, copy and paste the markdown file from `_project` to `_docs`

- Please note, to add the follow lines in the content of your doc copy, in order to generate the sidebar table of contents

```YAML
{:.no_toc}
* TOC
{:toc}
```


## Adding Collaborations

To create a new news page, you can create a new markdown file inside the `_collaborations` directory [here](https://github.com/maniaclab/maniaclab.github.io/tree/master/_collaborations). You may name the file anything you'd like (i.e. atlas.md, ligo.md, vertias.md), as long as it is an .md file.

Use the following example template:

```yaml
---
layout: collaboration_post
title: ATLAS
categories: [collaborations]
author: Jeremy Van
icon: /assets/collaborations/atlas_logo.png
excerpt: stuff about cern
---
- Very similar to adding a Projects page

- Where "icon" refers to the location of your project's image.

- Remember to add a photo to the '/assets/collaborations/' directory, which you'll be able to reference here. The page will still render the markdown with or without the image.
```

- Once you've created this collaboration's markdown, please also make a copy of this markdown file, and add it to the `/_docs` directory [here](https://github.com/maniaclab/maniaclab.github.io/tree/master/_docs), otherwise it will not come up in the search table of content (TOC) documentation. You may, quite literally, copy and paste the markdown file from `_collaborations` to `_docs`

- Please note, to add the follow lines in the content of your doc copy, in order to generate the sidebar table of contents

```YAML
{:.no_toc}
* TOC
{:toc}
```

## Editing History Timeline

To edit the content on the history timeline, you may add/edit the content located in `/_data/changelog.yml` or [here](https://github.com/maniaclab/maniaclab.github.io/blob/master/_data/changelog.yml) using the following format:

```YAML
- title: Maniac Lab
  label: STABLE
  date: Oct 21, 2017
  img: prc_building_2.jpg # Note that any photos must be placed/saved in `/assets/posts/`
  list:
  - The timeline is displayed from top to bottom as listed in the changelog.yml file
  - So despite given date, be sure to place your timeline accordingly
```


### Adding images
To keep things more organized, add timeline and post images to `/assets/posts/` directory, add project images to `/assets/projects/` directory, add collaboration images to `/assets/collaborations`.

To add an image to a post or page use the following codes:
Local image from `/assets/posts/` directory:
```yaml
{% include image.html img="girl.jpg" alt="Alt for image" caption="Girl on a rock" %}
```
External wide image with lightbox:
```yaml
{% include image.html img="https://source.unsplash.com/TT-ROxWj9nA.jpg" lightbox="true" alt="Alt for image" caption="Image in lightbox" %}
```

### Adding table of contents
Add the following code at the top of the post:
```
#### Sections in this article
{:.no_toc}
* TOC
{:toc}
```
`{:.no_toc}` exludes `#### Sections in this article` title from indexing in table of contents


## Installation

You can rebuild the site in many different ways, but the most common way is to run `bundle exec jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Install the dependencies with [Bundler](http://bundler.io/):

```bash
bundle install
```

Run the following to generate your site:
```bash
bundle exec jekyll serve
```

### Responsive Videos
Embed local videos:
```html
<video controls playsinline uk-video="automute: true">
    <source src="http://www.quirksmode.org/html5/videos/big_buck_bunny.mp4" type="video/mp4">
    <source src="http://www.quirksmode.org/html5/videos/big_buck_bunny.ogv" type="video/ogg">
</video>
```
Embed YouTube videos:
```html
<iframe src="http://www.youtube.com/embed/YE7VzlLtp-4?autoplay=0&amp;showinfo=0&amp;rel=0&amp;modestbranding=1&amp;playsinline=1" width="600" height="340" frameborder="0" allowfullscreen uk-responsive uk-video="automute: true"></iframe>
```


## Docs

To create a document post, just create a new page inside the root directory and add the following code in content:
```
{% include faqs.html %}
```

Create new doc post entries in `_docs` folder, similar to creating posts, but with following front matter settings:

```yml
---
layout: doc
title: Category hosting Setting up new domain and page
subtitle: This is optional doc subtitle
tags: featured development
author: peter
---
```

Sidebar navigation on docs post can edited in `_data/navigation_docs.yml`:

```yml
- title: Getting Started    # Section title
  docs:
  - home                    # Doc file name from _docs folder
  - quickstart
  - installation
  - windows
```

## Customization

To modify the primary color, open `/_sass/theme/variables.scss` and replace the color values e.g.:

```scss
// Main content
$color-main: #800000;
```

Further style customisation can be done in the following files:
```
/_sass/theme/mixins.scss
/_sass/theme/variables.scss
/assets/css/main.scss
```

### Google Analytics

To enable Google Anaytics, add the following lines to your Jekyll site:

```yaml
  google_analytics: UA-NNNNNNNN-N
```

Google Analytics will only appear in production, i.e., `JEKYLL_ENV=production`

### Google Map

To display Google map on contact page, add the following in your page content, replacing latitude, longitude and zoom values:

```yaml
{% include map.html latitude="40.6700" longitude="-73.9400" zoom="16" %}
```

### Contact Form (via FormSpree)

Submit the form and confirm your email address at [FormSpree](https://formspree.io/). Then add the following lines to contact page YAML Front Matter, replacing the email address:

```yaml
formspree:
    email: my_name@gmail.com
    redirect: /thanks/
```

### Update favicon

You can find the current favicon (favicon.png) inside the theme `/assets/img/` directory, just replace it with your new favicon.
