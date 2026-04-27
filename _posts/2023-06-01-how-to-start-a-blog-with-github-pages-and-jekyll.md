---
layout: post
title: How to Start a Blog with GitHub Pages and Jekyll
date: 2023-06-01 20:14 +0300
categories: [Blogging, Tutorial]
tags: [github-pages, blog, personal blog, jekyll, static website]
author: tremo
---

## How to Start a Blog with GitHub Pages and Jekyll

Starting a personal blog is a great way to share your ideas, projects, tutorials, and experiences with others.

If you are a developer, student, writer, or someone who wants a simple personal website, GitHub Pages is one of the easiest ways to publish a blog online for free.

In this guide, you will learn how to create a blog using GitHub Pages and Jekyll, choose a theme, publish your first post, connect a custom domain, and enable HTTPS.

## Why Use GitHub Pages for a Blog?

There are many platforms where you can start a blog, such as WordPress, Medium, Substack, and Ghost.

However, GitHub Pages is a good choice if you want a free, fast, and customizable website.

Here are some reasons to use GitHub Pages:

1. **Free hosting**  
   You do not need to pay for hosting your blog.

2. **Easy to manage**  
   You can manage your blog using Git and GitHub.

3. **Simple writing workflow**  
   You can write blog posts in Markdown.

4. **Customizable design**  
   You can use Jekyll themes and customize your blog as you like.

5. **Good for personal websites**  
   GitHub Pages is perfect for portfolios, project pages, and personal blogs.

## Useful External Links

Here are some helpful official and external resources:

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Jekyll Official Website](https://jekyllrb.com/)
- [Jekyll Installation Guide](https://jekyllrb.com/docs/installation/)
- [GitHub Pages Custom Domain Guide](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [Chirpy Jekyll Theme](https://github.com/cotes2020/chirpy-starter)
- [Let's Encrypt](https://letsencrypt.org/)
- [Markdown Guide](https://www.markdownguide.org/)

## Step 1: Choose a Jekyll Theme

The first step is to choose a Jekyll theme for your blog.

A theme controls the design and layout of your website. You can choose a simple theme, a portfolio theme, or a full blogging theme.

Here are some websites where you can find Jekyll themes:

- [Jekyll Themes](https://jekyllthemes.io/)
- [JekyllThemes.org](http://jekyllthemes.org/)
- [Jekyll-Themes.com](https://jekyll-themes.com/)
- [Jamstack Themes for Jekyll](https://jamstackthemes.dev/ssg/jekyll/)

One popular option is the [Chirpy theme](https://github.com/cotes2020/chirpy-starter/). It has a clean design, supports dark mode, and includes useful blogging features.

## Step 2: Create Your GitHub Pages Repository

After choosing a theme, create a GitHub repository for your blog.

If you are using the Chirpy theme, you can start from the official template:

[Create a blog from the Chirpy template](https://github.com/cotes2020/chirpy-starter/generate)

When creating the repository, use this format:

```bash
your-github-username.github.io
```

For example, if your GitHub username is `johnsmith`, your repository name should be:

```bash
johnsmith.github.io
```

GitHub Pages will publish your website at:

```bash
https://your-github-username.github.io
```

After the repository is created, GitHub Actions may automatically build and deploy your blog depending on the theme setup.

## Step 3: Clone the Repository

Now clone the repository to your local computer.

Open your terminal and run:

```bash
git clone https://github.com/your-github-username/your-github-username.github.io.git
```

Then go into the project folder:

```bash
cd your-github-username.github.io
```

Now you can edit your blog files locally.

## Step 4: Install Ruby and Jekyll

Jekyll requires Ruby.

Follow the official installation guide:

[Jekyll Installation Guide](https://jekyllrb.com/docs/installation/)

After installing Ruby and Jekyll, install the required gems for your blog:

```bash
bundle install
```

This command installs the dependencies needed by your Jekyll theme.

## Step 5: Update Your Blog Configuration

Most Jekyll blogs have a configuration file named:

```bash
_config.yml
```

Open this file and update the basic settings for your blog.

Common settings include:

```yaml
url: "https://your-domain.com"
avatar: "/assets/img/avatar.png"
timezone: "Asia/Riyadh"
lang: "en"
```

Here is what these options mean:

- `url` is your website address.
- `avatar` is your profile picture.
- `timezone` controls the date and time used on your blog.
- `lang` sets the language of your website.

You can also update your blog title, description, social links, and author information.

## Step 6: Run the Blog Locally

Before publishing your blog, test it locally.

Run this command:

```bash
bundle exec jekyll s
```

Then open your browser and visit:

```bash
http://localhost:4000
```

You should now see your blog running on your computer.

This helps you preview changes before publishing them online.

## Step 7: Add Your First Blog Post

Jekyll blog posts are usually stored inside the `_posts` folder.

A post file name should follow this format:

```bash
YYYY-MM-DD-post-title.md
```

Example:

```bash
2023-06-01-starting-a-blog-on-github-pages.md
```

At the top of every post, add front matter like this:

```yaml
---
layout: post
title: My First Blog Post
date: 2023-06-01 20:14 +0300
categories: [Blogging]
tags: [github-pages, jekyll]
---
```

After the front matter, write your article using Markdown.

## Step 8: Push Your Changes to GitHub

After editing your blog, save your changes and push them to GitHub.

Run these commands:

```bash
git add .
git commit -m "Update blog"
git push
```

GitHub will build and deploy your website.

After deployment, your blog should be available at:

```bash
https://your-github-username.github.io
```

## Step 9: Buy a Custom Domain

By default, your blog will use a GitHub Pages URL.

If you want a more professional website, you can buy a custom domain from a domain registrar.

Popular domain registrars include:

- [GoDaddy](https://www.godaddy.com/)
- [Namecheap](https://www.namecheap.com/)
- [Cloudflare Registrar](https://www.cloudflare.com/products/registrar/)

After buying your domain, you need to connect it to GitHub Pages.

## Step 10: Configure DNS Records

Go to your domain provider dashboard and open the DNS settings.

Add these `A` records for GitHub Pages:

| Type | Value |
|------|-------|
| A | 185.199.108.153 |
| A | 185.199.109.153 |
| A | 185.199.110.153 |
| A | 185.199.111.153 |

These records point your domain to GitHub Pages.

You can also add a `CNAME` record for the `www` version of your site:

| Type | Name | Value |
|------|------|-------|
| CNAME | www | your-github-username.github.io |

Replace `your-github-username` with your real GitHub username.

## Step 11: Configure the Custom Domain in GitHub Pages

After setting up your DNS records, go back to GitHub.

Follow these steps:

1. Open your blog repository on GitHub.
2. Go to **Settings**.
3. Open the **Pages** section.
4. Find the **Custom domain** field.
5. Enter your domain name.
6. Click **Save**.

Example:

```bash
example.com
```

GitHub will create a `CNAME` file in your repository.

This file tells GitHub Pages which custom domain should be connected to your site.

## Step 12: Enable HTTPS

After adding your custom domain, enable HTTPS.

In the GitHub Pages settings, check:

```bash
Enforce HTTPS
```

This makes your website load securely using SSL.

GitHub Pages can provide a free SSL certificate through Let's Encrypt.

HTTPS is important because it improves security and helps visitors trust your website.

## Step 13: Test Your Website

After updating DNS records, your website may not work immediately.

DNS changes can take a few minutes or several hours to update across the internet.

If you see a `404 error` or `Domain Not Found` error, wait for some time and check again.

You can also test your DNS records with this command:

```bash
dig YOUR-DOMAIN.COM +noall +answer
```

Example:

```bash
dig example.com +noall +answer
```

If your domain is correctly connected, you should see GitHub Pages IP addresses in the result.

## Common Problems and Fixes

### 1. My site shows a 404 error

This may happen if GitHub Pages has not finished deploying or if the repository name is incorrect.

Make sure your repository name follows this format:

```bash
your-github-username.github.io
```

Also check the **Actions** tab in your GitHub repository to see whether the build completed successfully.

### 2. My custom domain is not working

DNS records can take time to update.

Check that you added all four GitHub Pages `A` records correctly.

Also make sure your `CNAME` record points to:

```bash
your-github-username.github.io
```

### 3. HTTPS is not available yet

GitHub may need time to issue the SSL certificate.

Wait for a while and then try enabling **Enforce HTTPS** again.

### 4. The blog works locally but not on GitHub Pages

Check your `_config.yml` file.

For a user website, `baseurl` is usually empty:

```yaml
baseurl: ""
```

Also make sure your `url` value is correct.

## Final Thoughts

Creating a blog with GitHub Pages and Jekyll is a simple and affordable way to publish your content online.

You can write posts in Markdown, manage your website with Git, customize your theme, and host your blog for free.

Here is the process again:

1. Choose a Jekyll theme.
2. Create a GitHub Pages repository.
3. Install Ruby and Jekyll.
4. Configure your blog.
5. Write your first post.
6. Push your changes to GitHub.
7. Connect a custom domain.
8. Enable HTTPS.
9. Test your website.

If you want to start a personal blog, GitHub Pages and Jekyll are a great combination. They are free, flexible, and beginner-friendly once you understand the basic setup.
