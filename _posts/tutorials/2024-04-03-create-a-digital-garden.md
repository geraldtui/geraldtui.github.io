---
layout: post
title: Create a Digital Garden using Jekyll for FREE!
date: 2024-04-03 00:00 +13
categories: [Tutorials]
tags: [jekyll, chirpy, github]
---

Digital Gardens are fantastic tools for documenting your knowledge and expanding your skills. They act as dynamic portfolios that evolve alongside your learning journey.

Learn how to effortlessly create and share your own Digital Garden for free using Jekyll, the Chirpy Theme, and Github Pages.

---
## **Install Jekyll**

*Use [Jekyll Installation Guide](https://jekyllrb.com/docs/installation/) to install Jekyll and its dependencies


## **You will need these to customize and deploy Jekyll**
-  [GitHub Account](https://github.com/)
-  [Git](https://github.com/git-guides/install-git)
-  [VSCode](https://code.visualstudio.com/download) - or your preferred code editor


---
## **Use the Chirpy Theme to create and deploy the site**

### Step 1: Clone the [chirpy-starter](https://github.com/new?template_name=chirpy-starter&template_owner=cotes2020) GitHub repo

- Name your repository `<username>.github.io` (replace `username` with your github username).
- Select `Public` > `Create Repository`

![alt text](/assets/images/2024-04-03-create-a-digital-garden/image.png)
_Cloning Chirpy GitHub Repo_


### Step 2: Your website is up and running! 🎉
By the time you finish reading this line, your website would've already been published to GitHub pages and available on `https://<username>.github.io`

What are you waiting for? Go ahead and check it out 🙂

To check your site deployment status, go to the `Code` section of your GitHub repository. Find the deployment status at the bottom right of the page and click for more details.

![alt text](/assets/images/2024-04-03-create-a-digital-garden/screenshot-202403230117.png)
_Check Deployment Status_



---
## **Time to Customize**

### Step 1: Clone the repo to your local machine

```console
git clone git@<YOUR-USER-NAME>/<YOUR-REPO-NAME>.git
```

### Step 2: Install Chirpy Dependencies
```console
cd <repo name>
bundle
```


### Step 3: Update Configuration

In the root directory, edit `_config.yaml` to set these basic properties. While there are many other properties that can alter the site's configuration, these will suffice for now.

-  `title` = your website title (line 17)
-  `tagline` = line that will go under the title (line 19)
-  `url` = your github pages site (line 26)
-  `name` = used as author for all posts (line 37)

![alt text](/assets/images/2024-04-03-create-a-digital-garden/screenshot-202403232147.png)
_'_config.yaml' file_



### Step 4: Run Local Server

```console
bundle exec jekyll s
```

This should start the site on http://127.0.0.1:4000

![alt text](/assets/images/2024-04-03-create-a-digital-garden/screenshot-202403232158.png)
_Site after updating _config.yaml

---
## Create Your First Post
We write all posts in markdown language (`.md`) and keep them in the `_posts` folder.

### Step 1: Install Jekyll plugins
Jekyll has some cool plugins that help you whip up post templates super quick.

To your `Gemfile` in the root directory, add this line:
```console
gem 'jekyll-compose', group: [:jekyll_plugins]
```

Re-run the `bundle` command to install the Jekyll plugins:
```console
bundle
```

Run this to create a new post:
```console
bundle exec jekyll post "Hello World"
```

It will generate a new markdown file in `_posts`. Notice the naming convention `YYYY-MM-DD-hello-world.md`

### Step 2: Add These Properties To Your Post
Give it a category and some tags
```yaml
categories: [category]
tags: [tag1, tag2]
```

### Step 3: Add Markdown Text
```markdown
# Like, Comment, Subscribe
If you find value in this how-to doc. [Subscribe](https://www.youtube.com/@geraldtui8445?sub_confirmation=1) to my YouTube channel for more tutorials just like this one. 👍
```

### Deploy to GitHub
Commit and push your changes to your github remote repo to automatically deploy your site.

Checkout the official [Git Documentation](https://git-scm.com/doc) if you need.

## Markdown Help
Check the [Markdown Cheat Sheet](https://www.markdownguide.org/cheat-sheet/)

Checkout the official Chirpy docs for specific Chirpy syntax 👉 https://chirpy.cotes.page/posts/write-a-new-post/


## Useful VSCode Extensions

1. jekyll-post
    - For quickly creating posts using a template
2. Markdown All In One
    - For markdown shortcuts
3. Markdown
   - Automatically saves pasted images into a directory of your choice. I use `/asets/images`
   - In your VSCode Settings use this item in `Copy Files: Destination`
    ![alt text](/assets/images/2024-04-03-create-a-digital-garden/screenshot-202403240149.png)
