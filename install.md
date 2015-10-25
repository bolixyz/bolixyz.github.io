1) Create Repository 

Follow the instructions on [https://pages.github.com/](https://pages.github.com/) to create the the repository.

2) Install Jekyll

Following the link [https://help.github.com/articles/using-jekyll-with-pages/](https://help.github.com/articles/using-jekyll-with-pages/):
  1) Check ruby version `ruby --version`
  2) Install Bundler `sudo gem install bundler`
  3) Create a `Gemfile` at the repository directory with the following content:

```
source 'https://rubygems.org'
gem 'github-pages'
```

  4) Run `bundle install`

  First time run this command gave me error of 

```
An error occurred while installing RedCloth (4.2.9), and Bundler cannot continue.
Make sure that `gem install RedCloth -v '4.2.9'` succeeds before bundling.
```

  From this [blog post](http://jslim.net/blog/2012/12/08/os-x-failed-bundle-install/) by doing `sudo xcodebuild -license` solved my problem. It seems that after installation of Xcode I have never clicked agree on their license...

3) Running Jekyll

At the repository, run `bundle exec jekyll serve` and then the site can be accessed locally at [http://localhost:4000](http://localhost:4000).

4) Update Jekyll 

Simply at the repository directory run `bundle update`.


