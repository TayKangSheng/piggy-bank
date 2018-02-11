# Jekyll GitHub Pages Base

This is a base template for Jekyll on GitHub pages.

## Notable features
1. Jekyll version supported by Github Pages
2. Include GitHub Pages Gem
3. Extract Minima Theme out into this base template for straight forward custom templating and styling in this repository
4. Configured to run in `safe` mode by default. GitHub Pages runs your Jekyll project in safe mode

## How to use

1. Clone this repository
  ```sh
  $ git clone git@github.com:TayKangSheng/jekyll-github-pages-base.git
  ```

2. Rename the repository folder name to your preference. (`my_blog` is used as example)
  ```sh
  $ mv jekyll-github-pages-base my_blog
  ```

3. Start your page server
  ```sh
  # Change directory
  $ cd my_blog
  # Install all dependencies
  $ bundle install
  # Start blog server
  $ bundle exec jekyll serve
  ```

4. Open your preferred browser and go to http://localhost:4000

5. Start editing your project files and refresh the browser page to see the updated changes

## How to host on GitHub

For official guide, please visit https://pages.github.com/

1. Create a new repository on GitHub named **username.github.io**

  ![image one][image_one]

2. Upload local repository to the remote repository you have just created  
  (Remember to change username to your own GitHub username)

  ```sh
  $ git remote add origin git@github.com:<username>/<username>.github.io.git
  $ git push -u origin master
  ```

3. Open your browser and go to **https://username.github.io** to see your hosted website!

  ![image two][image_two]

## Recommended next steps
1. Style your page and extract `_includes`, `_layouts`, `_sass`, `assets` and `script` into a separate gem.

## References
- https://pages.github.com/
- https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/#step-3-optional-generate-jekyll-site-files
- https://jekyllrb.com/docs/quickstart/
- https://jekyllrb.com/docs/usage/

[image_one]: https://github.com/TayKangSheng/jekyll-github-pages-base/blob/master/README/README_IMG_1.png
[image_two]: https://github.com/TayKangSheng/jekyll-github-pages-base/blob/master/README/README_IMG_2.png
