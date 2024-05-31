---
layout: default
title: Katherine Maack
---

# How to create a static website with a custom domain
\
**Estimated read time**: 4 minutes\
**Estimated task time**: 60 minutes

## Tools
* **Github Pages** - static site hosting
* **AWS route53** - domain registration and hosting
* **Jekyll** - our static site generator

## Steps
### [Prerequisites](#install-prerequisites)
### [Create a Github Pages static site](#create-github-pages-site)
### [Setup a domain with AWS Route53](#route53-configuration)
### [Point the custom domain to the Github Pages site](#connect-domain-to-site)
## Details
### Install Prerequisites
* install [Ruby](https://www.ruby-lang.org/en/documentation/installation/)
* install [Bundler](https://bundler.io/)
* install [Jekyll](https://jekyllrb.com/docs/installation/)
* install [Git](https://docs.github.com/en/get-started/getting-started-with-git/set-up-git)

### Create Github Pages Site
We'll be using [these directions](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)
for reference. 
1. Create a github repository
2. Enable Github Pages for your new repository
![](https://github.com/the-star-thrower/the-star-thrower.github.io/blob/master/assets/images/enable-github-pages-for-repo.jpg?raw=true)
3. Create a new jekyll site in the top-level directory of your new project. You should at most have `CNAME`, 
`LICENSE`, and `README.md` files so running `jekyll new --skip-bundle . --force` shouldn't overwrite any files, despite
the warning.
4. Follow the directions to update the Gemfile and run `bundle install`. You may need to edit the version of the `Jekyll`
gem in addition to the `github-pages` depending on Github's [dependency version](https://pages.github.com/versions/) for Jekyll.
5. Follow the remaining directions on testing your site locally and committing your work. 

### Route53 Configuration
1. Create an AWS account or login
2. Navigate to the Route 53 service
![](https://github.com/the-star-thrower/the-star-thrower.github.io/blob/master/assets/images/route-53-console.png?raw=true)
3. Choose a domain
![](https://github.com/the-star-thrower/the-star-thrower.github.io/blob/master/assets/images/choose-domain.png?raw=true)
Today i'm favoring being cheap and carbohydrate polymers are on my mind, so i'll go with the $9/year `.de` TLD
and a snappy domain to talk about oligosaccharides.
4. Step through the domain registration process
5. We now have a registered domain. There will be some latency while AWS configures this domain in its backend, 
so after a few minutes, be sure to confirm you can see your domain in `Route 53` -> `Domains` -> `Registered Domains`. 
Our next step is to ensure the domain directs us to our Github Pages-hosted site.

### Connect Domain to Site
Now that we have our Github Pages site setup and our Route 53 domain registered, we want to ensure our new domain
directs us to our Github Pages site. For example, by default the domain for this website is `the-star-thrower.github.io`, 
so I configured the custom domain `katherinetaylormaack.com` to also direct to this Github Pages-hosted site. We'll be using
[these directions](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site) as a reference.
1. In your Github Pages repository, navigate to `Settings` -> `Pages` and under `Custom domain`, type your custom domain and save.
Even if you receive the following error, Github should still successfully create a CNAME file in your project.
![](https://github.com/the-star-thrower/the-star-thrower.github.io/blob/master/assets/images/error-custom-domain.png?raw=true)
2. In the Route 53 interface, open `Hosted zones`. Select your custom domain then select `create record`. Create an `A` 
record pointed to the following addresses:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```
![](https://github.com/the-star-thrower/the-star-thrower.github.io/blob/master/assets/images/create-a-record.png?raw=true)
3. After clicking `create record`, you should be able to navigate to your website and see the simple Jekyll-based
website you created. It should look something like [this](http://oligosacchari.de/).
4. That's it, you're done!

![](https://gifdb.com/images/high/you-did-it-happy-willy-wonka-and-chocolate-factory-2s1venj99oc71li9.gif)

### Extra Credit
If you'd like to dress up your new website, try following [these directions](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll)
to add a custom theme to your new site!

## Problems and their Solutions
### <span style="color:#EA5F41">Problem: running `gem install bundler` on MacOS results in:</span>
```
You don't have write permissions for the /Library/Ruby/Gems/${VERSION} directory.
```

### <span style="color:#479F3F">Solution</span>
MacOS ships with its own copy of Ruby, and does not want the user to have edit privileges for this version.

Let's instead install a separate version of Ruby that won't interfere with the builtin MacOS
version, and we'll update the path variable to use the new Ruby version. We'll be using `rbenv`, but other options
include `chruby` and `RVM`. 
* install [rbenv](https://github.com/rbenv/rbenv)
* `rbenv init`
* You may need to modify your shell initialization files to bootstrap rbenv functionality. In my case, 
I use fish shell so I appended `status --is-interactive; and rbenv init - fish | source` to 
`~/.config/fish/config.fish`.\
**Note**: `rbenv` should provide directions for doing this with your specific shell.
* `rbenv install {$VERSION}` (`rbenv install -l` for available versions)
* verify you're using the newly installed Ruby version with `ruby -v`
* re-run `gem install bundler`

### <span style="color:#EA5F41">Problem: running `bundle exec jekyll serve` results in:</span>
```
/Users/maackk/.rbenv/versions/3.3.2/lib/ruby/gems/3.3.0/gems/jekyll-3.9.5/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
	...[stack trace]
```

### <span style="color:#479F3F">Solution</span>
run `bundle add webrick`

credit and explanation: [@argilo on github](https://github.com/jekyll/jekyll/issues/8523)