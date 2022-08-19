# SAIFAS Jekyll app-boilerplate documentation
#jekyll-app-boilerplate-documentation

[Jekyll app-boilerplate](https://github.com/JekyllGO/saifas-ssg-jekyll-app-boilerplate)

This repository contains an empty Jekyll project and a README that describes a complete deployment of Jekyll on your Linux operating system for further work with it.

## Preparatory work

Before you start working with Jekyll you need to install the following components on your system.

1) Install Ruby [https://www.ruby-lang.org/en/downloads/]
2) Install Ruby Gems [https://rubygems.org/pages/download]
3) Install GCC [https://gcc.gnu.org/install/]
4) Install Make [https://www.gnu.org/software/make/]
 
## Creating project

1) Install Jekyll Bundler
```sh
gem install jekyll bundler
```
2) Create new project
```sh
jekyll new myblog
```
3) Run project
```sh
bundle exec jekyll serve
```

## Our Jekyll Application Architecture

### 1. Basics of building a structure

The application structure starts with the app folder, which contains the app-build-static and jekyll-stack folders
Example:
```
app/
  app-build-static/
  jekyll-stack/
```
`app-build-static` is the temporary folder where the project is being built. The path to the build location is stored in the `destination` variable in the config file.
`jekyll-stack` is the main repository where the entire project structure is contained.

The "skeleton" jekyll-stack should contain the following folder structure:
```
_data/
_includes/
assets/
  graphics/
    fonts/
    images/
  scripts/
  styles/
    css/
contents/
templates/
  _layouts/
```
The path to the layout is specified in the `layouts_dir` variable. 

### 2. Routing
Routing is represented by a folder structure. The `contents_root_path` and `contents_pages_path` variables are defined in config.yml , which describe the path from the page structure of our Jekyll app. The contents of pages are folders that describe the site's routing.
Example: 
```
pages/
  index.md
  powerbi/
    index.md
    custom-visuals/
      index.md
      table/
        index.md
        reports/
          index.md
            ...
```
Each of the folders contains the index.md file, which contains all the variables used on the page and a pointer to the html layout used.
Example index.md file:
```
---
layout: home
title: 'SAIFAS BI'
pageUrl: './'
pageName: 'SAIFAS BI'
headline: 'Our experience'
permalink: /index.html
---
```
Also the md file contains the `permalink` variable which describes the url path to the page.
The layout contains the html layout of the page using the variables described in the md file. Layouts are in /_layouts.

### 3. Data management
Data organization can have two options: using collections and using jekyll-data. We use the second option.
The /_data folder contains subfolders describing any entity. The elements inside are .yml files, each of which describes a specific instance of an entity and contains all the variables associated with it.
Structure example:
```
_data
  /powerbi
    calendar.yml
    legend.yml
    table.yml
    ...
```
Yaml file example:
```
minimized:
  title: Project
  description: Power BI Custom Visual
  link: https://github.com/SAIFAS-BI/saifas-pbi-cv-project-support-source/issues
  detailsLink:
  image: /assets/graphics/images/content/saifas-bi-powerbi-custom-visuals/saifas-bi-pbi-cv-project-120px-120px.png
detailed:
```
The display of elements is carried out using the cycle described in the md file of the page.
Example of displaying jekyll-data files:
```
{% for pbi-cv_hash in site.data.powerbi.custom-visuals %}
{% assign pbi-cv = pbi-cv_hash[1].minimized %}
  {% include powerbi/custom-visual/card.html %}
{% endfor %}
```
The cycle displays templates that are stored in the _includes folder. These are html files that contain the layout of one or another element, and also use the variables described in yml files.
The _includes structure also repeats the _data structure. The _includes folder contains entity subfolders that contain template elements for the parent entity.
