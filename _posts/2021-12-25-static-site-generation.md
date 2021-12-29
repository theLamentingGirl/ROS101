---
layout: post
author: Harini
---

# How to generate and deploy Static site with ease?

## Jekyll

Jekyll is a static site generator built on Ruby programming lang. this lets your create your website in markdown as well as html. To get started, first download ruby

### In Ubuntu, to install jekyll:
1. `sudo apt-get install ruby-full` 
2. `gem install jekyll` 

### To check if ruby and jekyll have properly installed: 
1. `ruby -v`
2. `jekyll -v`

### To create a jekyll site:
1. `jekyll new <new_site>`
2. `sudo apt install bundler`
3. `bundler exec jekyll serve` - just this to get the server running 

This creates a local host of your site with jekyll default theme of 'minima'. That can be changed by going to `Gemfile` under <new_site>

### Contents of <new_site> directory that is created:
1. `_posts` - containss the posts that you want to deploy on your site
2. `_site` - the output of your site and things needed in the output site goes here. This gets updated for you.
3. Gemfile - specifying ruby dependencies

Ignore others for now

### Creating 1st post in Jekyll site
Under `_posts` we can see a "Welcome-to-jekyll" default post already there. To create a new post like that, we create a new .md file with the following naming convention:\
`yyyy-mm-dd-title-of-your-post.md` 
This convention is important to be followed for every post because the address where your post will be stored depends on the date. It gets stored in the local address like `/yada/yada/yyyy/mm/dd/title-of-your-post`

The title-of-your-post.md should always have a .yml beginning, called front matter, to present it the look the static webpage needs to have.

it can be very simply mentioned at the start of the .md doc as \
```yml
---
layout: "post" 
title: "my title"
# <can add more here>
---
```

Add the .md content after this

### Create drafts
Instead of posts, to have a temporary idea of what the post should look like, create drafts. For this we create a new directory called `_drafts` where we save all the drafts. The usefulness of this is that the drafts don't appear when we launch our site. When we launch the site with the specific command `jekyll serve --draft` this is when the drafts will be showed on our website. \
The format of storing a draft can be  just `your-draft-name.md` without the yyyy-mm-dd at the beginning. The default date that will be displayed is the last modified date

### Creating pages
This is very simple. Just create a <your-page>.md and that will be a page. Make sure to include front matter. Every page/post/draft needs front matter.

### Basic things to be included in front matter
1. title: "my-title"
2. date: yyyy-mm-dd time
3. categories: "my-cat" - this includes address that page should have
4. permalink: "/link/bleh" This is important to set because we saw before that changing the date and title, changes the file location. This stabilises it to that unique address. To include category `/:categories`


### Creating default front matter
WE make changes to the _config.yml file in our website directory. Add
```yml
defaults:
    - 
        scope:
            path: "proj"
        values:
            layout: "post"
```

## Changing Jekyll theme
- Search for jekyll themes at [rubygems.org](rubygems.org).
- follow what is told in the Github documentation of these themes. Most ask to make changes in _config.yml file and Gemfile.\
    In the _config file add `
  theme: <theme-name> (or)
  remote-theme: user/<remote-theme-name>
  `
    In the gem file add `gem "<theme-name>"` or `gem "jekyll-remote-theme"`\
Execute these commands in the terminal:
1. `bundle install` - this is the install gem dependencies
2. `bundle exec jekyll serve` - this is the usual command to launch the website

The problem that might arise is because of the layout type not existing in your new theme. So, in the GitHub repo of the jekyll site, check what are the layouts available in `_layouts` folder.

## Creating new custom layouts
- create a new directory called `_layouts` in website directory
- make an <your-layout>.html document where all design details can also be embedded using css
- To make your content of .md file appear with <your-layout>.html, must include 
  content written within 2 curly braces.
- We can also add yaml front matter in the beginning of the html doc to make wrapper designs.
    **Making wrapper/ hierarchy of layouts (nested layouts):**
    - create level-3.html and add content in the body and front matter with `layout: level-2` 
    - create level-2.html and add content in the body and front matter with `layout: level-1`
    - create level-1.html and this has no yaml front matter
    - give the innermost nested `layout: level-3` to the actual post

### Accessing variables in custom layouts
- To view title(or whatever is in the yml in .md file) from the .md page - {{ page.title }}
- To view layout details, eg. the author listed in the yml front matter of layout - `{{ layout.author}}`
- More variables can be found at [https://jekyllrb.com/docs/variables/](https://jekyllrb.com/docs/variables/) 

## Abstracting designs to be applied everywhere
Includes lets us have reuseable designs that needs to be applied at multiple places. The difference between includes and layout - layouts helps in the organisation of information and includes helps to reuse the design components. Refer this website for more information: [https://jekyllrb.com/docs/includes/](https://jekyllrb.com/docs/includes/)

Note: Use of includes isn't very obvious to me. I'll have to use it somewhere to get a hang of it.

Take advantage of html loops and conditionals. Use css when needed too!




