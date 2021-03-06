---
title: Releasing sblog
date: 2020-07-15 20:50:20
tags: ["automation","blogging"]
---

Today, I wrote a very simple utility that helps me kickstart new blog posts. In fact, this is the first blog post bootstrapped with the utility.

I have named it `sblog` - short for "Start a new blog post"

It just saves a few seconds of my time and frees me from manually typing timestamps in my blog post headers everytime.

I have been continously trying to bring down the friction in starting and publishing blog posts for a long time. This is probably a small step in that path.

My first effort was full automation of writing blog posts via Github Editor and publishing it by commiting the markdown file. You could read all about it from here - https://vishnubharathi.codes/blog/auto/

After that I have been using Bookmarks for creating new post and uploading new image to my blog.

![blog-bookmarks](/images/blog-bookmarks.png)

This used to give me a blank blog post page like 

![blank-blog-post](/images/blank-blog-post.png)

I have to manually type in the filename like `releasing-sblog.md` for creating a blog post like this and fill in the content with the following YAML front matter.

```yml
---
title: Releasing sblog
date: 2020-07-15 20:50:20
tags: ["automation","blogging"]
---
```

That's some effort and I don't feel very fluid about my workflow.

So today, I am releasing a small utility called `sblog`.

Here is how it works,

When I want to start a new blog post, I run

```bash
 sblog "Releasing sblog" "automation,blogging"
 ```
 
 That's it! It magically opens the browser with autogenerated content to help me get right to the content of the blog post.
 
 ![sblog-screenshot](/images/sblog-screenshot.png)
 
 The source is available in the open - https://github.com/scriptnull/sblog
