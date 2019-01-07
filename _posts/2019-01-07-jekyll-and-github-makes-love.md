---
layout: single
title: "Jekyll + Github: Free website hosting tutorial"
excerpt: "Create your own blog/portofolio using these technologies, with no hosting fee!"
permalink: /:title
---

## Requirements:

1. Terminal
2. Check whether you have Ruby 2.1.0 or higher installed:
    ```
    $ ruby -v
    ruby 2.x.x
    ```
3. Install Bundler:
    ```
    $ gem install bundler
    ```
4. Create a local repository for your Jekyll site. (Jump to step 1 if this is done)

    * Install git, see [Set up Git](https://help.github.com/articles/set-up-git/)
    * Open terminal
    * Initialize a new Git repo for your jekyll site:

    ```
    $ git init <GitHubUsername>.github.io
    Initialized empty Git repository in /Users/horseforce/andrehatlo.github.io/.git/
    # creates a new file directory on your local computer, Initialized as a Git repo
    ````

    * Then change directory to the new repository created:
    ```
    $ cd <
    ```    


## Step 1: Install Jekyll using Bundler

1. Check if you have a Gemfile in your newly created repo

    ```
    $ ls
    Gemfile
    ```

    * If you have a Gemfile, skip to step 4.

2. Open up your favorite text editor, mine is [Atom](https://atom.io/), and add these lines to a new file

    ```
    source 'http://rubygems.org'
    gem 'github-pages', group: :jekyll_plugins
    ```

3. Save the file as `Gemfile`, in the root directory of your local Jekyll repository. Now skip to step 5 to install Jekyll.

4. **Only** if you already have a Gemfile. Open your favorite editor, mine is [Atom](https://atom.io/), and add these lines to your Gemfile:

   ```
   source 'https://rubygems.org'
   gem 'github-pages', group: :jekyll_plugins
   ```

5. Install Jekyll and other dependencies from the GitHub Pages gem by running:

   ```
   $ bundle install
   Fetching gem metadata from https://rubygems.org/......
   Fetching version metadata from https://rubygems.org/...
   Fetching dependency metadata from https://rubygems.org/..
   Resolving dependencies...
   ```

## Step 2: Run your Jekyll site localy

You can generate site files for a basic Jekyll template site in your local repository.

1. If you don't already have a Jekyll site localy on your computer, create a Jekyll template site in a new directory:

    ```
    $ bundle exec jekyll _3.3.0_ new <new-jekyll-site-repo-name>
    New jekyll site installed in /Users/horseforce/<new-jekyll-site-repo-name>.
    ```

2. Navigate into your new site directory and edit your Gemfile by removing the following line:

    ```
    $ 'jekyll', "3.3.0"
    ```

3. In the same Gemfile, delete the `#` at the beginning of this line:

    ```
    $ gem "github-pages", group: :jekyll_plugins
    ```

4. Initialize the directory as a Git repo

    ```
    $ git init
    ```

5. Connect your remote repo on GitHub to your local repo for your site.

    ```
    $ git remote add origin http://github.com/<username/organization-name/your-remote-repo-name>
    ```

6. To edit the Jekyll site, open your jekyll local repository in an text editor. Make as much changes as you want and save them.

7. Preview changes locally by running a Jekyll command yo build your site:

  * Navigate to your site root directory:

    ```
    $ cd <GithubUsername>.github.io
    ```

  * Run the following command:

    ```
    $ bundle exec jekyll serve
    Configuration file: /Users/horseforce/andrehatlo.github.io/_config.yml
                Source: /Users/horseforce/andrehatlo.github.io
           Destination: /Users/horseforce/andrehatlo.github.io/_site
     Incremental build: disabled. Enable with --incremental
          Generating...
                        done in 0.309 seconds.
     Auto-regeneration: enabled for '/Users/horseforce/my-site'
    Configuration file: /Users/horceforce/andrehatlo.github.io/_config.yml
        Server address: http://127.0.0.1:4000/
      Server running... press ctrl-c to stop.
    ```

  * In your browser, preview your local Jekyll site at `http://localhost:4000`

8. Add or stage your changes:

    ```
    $ git add .
    ```

9. Commit it

    ```
    $ git commit -m "some stuff changed"
    ```

10. Push your changes to your remote repository on GitHub

    ```
    $ git push -u origin master
    ```

# Step 3: Make github host the site for you

1. Navigate to your repository. Which should be called something like `<username>.github.io`

  * If your repository is called something else, we can change it at the next step.

2. Enter into the repository settings.

  * First thing you see is `Repository name`, edit it so that it maches the syntax `<username>.github.io`.

  * Scroll down to `GitHub Pages`

3. At `GitHub Pages`, there can be all kind of messages. But hopefully it should say:


> :heavy_check_mark: Your site is published at [http://www.andrehatlo.github.io](http://andrehatlo.com)

  * If not, there is possibly something wrong with your repository or github setup.

4. Just keep tweaking your website and check out all the different themes out there! I will be coming with more tutorials about Jekyll in the future :smile:


### Hope this helps, it is my first blog post (I know its not the best). But i'll try to excel as fast as possible by writing MANY MANY more! :wave: :wave:
