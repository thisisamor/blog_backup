---
title: Test Wrap Up
date: 2023-04-14 22:08:33
categories: Test
tags: 
    - test 
    - cheat sheet
description: I think it's a proper time for me to summarise a little. It's always good to write something down... Because my memory is poor. 

---

<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;">I think it's a proper time for me to summarise a little. </small>
<small style="opacity: 0.7;">It's always good to write something down... Because my memory is poor. </small>

---

### Hexo (and Jekyll)

#### Hexo

Hexo is a static site generator that is built on Node.js. 

It does not require a database or server-side scripting language. Instead, it generates websites by processing plain text files written in a markup language (such as Markdown or HTML), and then applying a template (written in a templating language called EJS (Embedded JavaScript), which allows one to generate dynamic content using JavaScript code) to generate static HTML, CSS, and JavaScript files. 

- `hexo generate`
    Hexo reads all the Markdown files, converts them into HTML and combines them with the theme files to generate a complete HTML website. During the generation process, Hexo also copies any other static assets (eg: images and CSS files) into the appropriate locations in the final site directory. 

- `hexo delpoy`
    Hexo takes the generated files and pushes them to the remote repo using Git. This process involves creating a new commit with the updated files and pushing it to the remote repo. After that, the website is deployed and accessible via its URL. 

#### Jekyll

Also a static site generator, wirtten in Ruby. 

How Hexo and Jekyll handle content:
- Hexo uses a file-based system, where each piece of content is stored as a file in a directory structure that mirrors the structure of the website. 
- Jekyll uses a collections-based system, which groups related pieces of content together and organises them by category, tag, etc. 

How Hexo and Jekyll generate websites:
- Hexo uses a caching system to minimize the amount of time it takes to generate a website. (It was designed to be fast and simple.)
- Jekyll generates the website from scratch every time, which can be slow if there is a lot of content.

Apart from the fact that Jekyll has a larger community, they seem pretty similar to me in terms of using their commands (eg: `jekyll build`, `jekyll deploy`, `jekyll-ftp`, etc). 

#### Programming Languages and Frameworks

<small style="opacity: 0.7; font-style: italic">"Hey gpt, what web development languages do hexo and Jekyll use? And also, give me a brief introduction on other popular languages." </small>

Hexo is built with the Node.js runtime environment. 

Node.js is a JavaScript runtime that allows developers to write server-side code using JavaScript, which is traditionally a client-side language. Hexo also supports the use of Markdown for content creation and offers a powerful plugin system for extending its functionality.

Jekyll is built with the Ruby programming language. 

Ruby is a dynamic, object-oriented programming language that is used for web development, among other things. Jekyll also uses the Markdown language for content creation and supports front matter, which allows for the addition of metadata to each page.

Other popular web development languages:
- HTML: HyperText Markup Language is the standard markup language used for creating web pages and other online content. It provides the structure and content of web pages.
- CSS: Cascading Style Sheets is a style sheet language used for describing the presentation of a document written in HTML or XML. It provides the layout, colors, fonts, and other visual aspects of web pages.
- PHP: Hypertext Preprocessor is a server-side scripting language used for web development. It is used for creating dynamic web pages and can be embedded in HTML.

---

### Workdiary

#### Setup

1. Install Git

2. Install Node.js

3. Install Hexo
    ```
    npm install -g hexo-cli
    ```

4. Initialise local directory
    ```
    hexo init <your_new_blog_directory_name>
    cd <your_new_blog_directory_name>
    npm install
    ```

5. Create github repo
    Set repo name as `<username>.github.io`. 

6. Generate SSH (secure shell protocol)
    Run the following commands in git bash: 
    ```
    git config --global user.name "yourname"
    git config --global user.email "youremail"
    ssh-keygen -t rsa -C "youremail" 
    ```
    Passphrase is not necessary. 
    Default directory would be `C:\Users\<username>/.ssh/id_rsa`. 

7. Add SSH in github
    Copy the key generated in `id_rsa.pub`. 
    Go to `https://github.com/settings/keys`, add new SSH key. 
    Should be identified in git bash now by running `ssh -T git@github.com`. 

8. Configuration
    Open `_config.yml` and add the following code to the deploy section at the bottom. 
    ```
    deploy:
      type: git
      repo: git@github.com:<username>/<username>.github.io.git   // repo SSH can be copied from github
      branch: main
    ```

9. Deployment
    To deploy for the first time, run `npm install hexo-deployer-git --save`. 
    After that, simply use the following commands:  
    ```
    hexo cl
    hexo g -d
    hexo s
    ```

#### Theme

1. Change theme
    Choose theme on [Hexo theme website](https://hexo.io/themes/), clone the repo into `/themes` directory. 
    Change theme name in `_config.yml` to the name of the theme (eg: `theme: next`). 

2. Theme Customisation
    Made obvious changes to `_config.yml` in my root dir. 
    For theme `_config.yml`, i made following changes. 
    - scheme
    - darkmode
    - menu
        ```
        menu:
          Home: /
          archives: /archives
          categories: /categories
          tags: /tags
          About: /about
        ```
    - menu_settings
    - avatar
        ```
        url: /images/amor_avatar.gif
        ```
    - social
    - icon between year and copyright info
    - excerpt_description and read_more_btn
    - post_meta
    - font (I used [Lora](https://fonts.google.com/specimen/Lora?query=lora) and [Noto Serif SC](https://fonts.google.com/noto/specimen/Noto+Serif+SC?query=noto+serif+sc), serif was the default one)
        ```
        family: Lora, Noto Serif SC, serif
        ```
    - mermaid
    - math

    
    In `arhive.njk`: Deleted `set cheers` part. 

    In `tags.njk`: Changed `min_font: 16, max_font: 16` (all tags would have the same size). 

    In `tag-cloud.styl`: (all tags would have the same colour.)
    ```
    .tag-cloud {
      text-align: center;
      a {
        display: inline-block;
        margin: 10px;
        color: #333; /* Set a single color value for all tags */
      }
    }
    if (hexo-config('darkmode')) {
      @media (prefers-color-scheme: dark) {
        .tag-cloud a {
          color: #fff; /* Set a different color for tags in dark mode */
        }
      }
    }
    ```

    In file `post.njk`
    - Modified `post-meta-container` (the description would only be shown on home page). 
    - Changed `<p style="opacity: 0.5">{{ post.description }}</p>` (the description would be faded on home page). 

#### Backup

I DON'T THINK THAT I HAVE DONE THIS PROPERLY. 

I first tried to commit my local directory to a new branch in the same repo to which Hexo was deployed, but somehow ended up with a new repo under my other account (the one I use for college work). (*How could this even happen? I mean I have logged into my account both in vs code and git bash I simply can't see why*.) 

After initialising the git file I decided that I'm going to do the simpliest thing and just push everything to a new repo. This is the part that I believe is not the best solution. However, I thought about this: *if* one day I were to lose my computer, or it broke down, or, I simply wanted to replace it with a new one, with all the softwares and systems that would need to be reinstalled, I'm not going to make it a hard job when comes to restore my blog (*happily found myself an excuse to be stupid*). 

Anyway, the following are the commands I used to back up. 

```
git init
git add . 
git commit -m "commit message"
git remote add origin https://github.com/thisisamor/blog_backup
git push -u origin main
```

(The gitignore file in `/themes/next` folder needs to be deleted, so that the customised theme could be backed up.)

When restoring the local blog folder, simply clone the whole repo and run `npm install`. Then everything should be back. (If using new computer, regenerate and add new SSH key to github acc, obvi.)

I tested and it worked. 

---

### Troubleshooting

#### Capitalisation

Changing the category name from `test` to `Test` caused the "404 page not found" error. It is due to the fact that github is not case sensitive, while the URLs in HTTPS are. 

This problem was supposed to be fixed by changing `ignorecase = true` into `ignorecase = false` in the config file in `\.deploy_git\.git`. 

I tried, didn't work. 

I removed the files from my posts folder, deployed my blog, then I put them back, deployed again. Issue fixed. 

---

### Cheat Sheet

#### Commands

- The obvious ones in Hexo
    ```
    hexo cl
    hexo g -d
    hexo s
    hexo new post "new-post-name"
    hexo new draft "new-draft-name"
    hexo publish "new-draft-name"
    ```

- To back up
    ```
    git add . 
    git commit -m "commit message" 
    git push
    ```

#### Code

Some HTML tags I used for styling: 
- Read more button
    `<!-- more -->`
- Faded
    `<p style="opacity: 0.7;"> text </p>`
- Small faded
    `<small style="opacity: 0.7;"> text </small>`
- Small italic faded 
    `<small style="opacity: 0.7; font-style: italic"> text </small>`
- Coloured text
    `<span style="color:red;"> coloured </span>`
- Highlighted text
    `<mark> highlighted </mark>`
    `<mark style="background-color: #<hex_code>;"> customised highlight </mark>`
- Icon
    `<i class="fa-regular fa-icon"></i>`
- Hyperlink
    `<a href="URL" style="text-decoration: none;"> text </a>`
- Picture size and caption
    ```
    <figure>
      <img src="image source" width="width in px" alt="alt text">
      <figcaption>This is a caption. </figcaption>
    </figure>
    ```

#### Useful Websites

<i class="fa-solid fa-link"></i> [Hexo setup manual on CSDN](https://blog.csdn.net/sinat_37781304/article/details/82729029?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168150927616800215012048%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168150927616800215012048&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-82729029-null-null.142^v83^koosearch_v1,239^v2^insert_chatgpt&utm_term=hexo&spm=1018.2226.3001.4187)

<i class="fa-solid fa-link"></i> [Font Awsome](https://fontawesome.com/search?o=r&m=free)

<i class="fa-solid fa-link"></i> [Google Fonts](https://fonts.google.com/) 

<i class="fa-solid fa-link"></i> [Hex Color Code](https://www.color-hex.com/)

---

<p style="opacity: 0.7;">End Notes: 

<small style="opacity: 0.7;">Well, ChatGPT helped a lot too... particularly when I'm unsure about a specific line of HTML code. I guess the copilot function would be quite useful for this purpose. </small>
