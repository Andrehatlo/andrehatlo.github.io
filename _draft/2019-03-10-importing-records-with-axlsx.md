---
layout: single
title:  "Roo gem: Importing records using excel"
permalink: /importing-records-with-roo/
excerpt: "Importing records to database with roo and excel"
header:
  image: '/assets/images/axlsx-excel.jpg'
  teaser: '/assets/images/axlsx-excel.jpg'
author_profile: false
comments: true
draft: true
---

Using the Roo gem, we can parse excel sheets and import rows of attribute details and update our database.

## The gem

First of all, include the `roo` gem in your `Gemfile`:

```ruby
//Gemfile
gem 'roo'
```

`bundle` that shit so you install the gem and your ready to parse and roll.

## The model

We need an excel file with headers that are called the same as your record attributes.

For example a `Car` model:

```ruby
Table: cars
- id            integer
- licence_numb  string  
- brand         varchar
- year          string
- color         varchar
```

## The sheet


Since i want to create new records as well as update them, i will leave out the `id` attribute and rather target them on their unique licence number (`licence_numb`).

An excel spreadsheet for this model would look like this:

<img src="/assets/images/excel/car-model-xl.png" alt="react-native prop error" width="600">

## The parser
