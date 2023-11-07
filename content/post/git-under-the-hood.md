---
title: "Git Under the Hood"
date: 2023-11-03T08:55:26+07:00
# weight: 1
# aliases: ["/first"]
tags: ["git"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: true
hidemeta: false
comments: false
description: "Desc Text."
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---
## Introduction
If you are a developer you may at least one time use version control such as: GIT, BITBUCKET, SVN... and i think GIT will be the first choice among these tools. Have you ever wonder why GIT is so common like that? What is the power behind GIT?, etc. Today you and I will discover the secret of GIT and after this article you won't scare GIT anymore. Let's get started 
## Git object types
When talk about GIT, we should only care about three things: **BLOB**, **TREE** and **COMMIT**. All of them will be compressed into a `SHA-1` before being stored to git database. 
> SHA stands for Secure Hash Algorithm. A SHA creates an identifier
of fixed length that uniquely identifies a specific piece of content.
SHA-1 succeeded SHA-0 and is the most commonly used
algorithm. Wikipedia (http://en.wikipedia.org/wiki/SHA1) has more on the
topic.
>
Let's dive deep into each type
### BLOB
In Git, **BLOB** store content of a file. I must emphasize that a content of a file not the file's name. It means if you have 2 files with the same content but in the different place, Git only store 1 blob. For instance, I create 2 files `test1.txt` and `test2.txt` with the same content is `Hello world`
![avatar](/photos/git-under-the-hood/image-1.png)
Let's `git add` to see how git store those files. 
![avatar](/photos/git-under-the-hood/image-2.png)
![avatar](/photos/git-under-the-hood/image-3.png)
As we can see, when I add those files, Git only store 1 blob with SHA-1 is `802992c4220de19a90767f3000a79a31b98d0df7` in `.git/objects`. We will talk more about `.git` folder in the next section but basically it's Git's database. Let's try to add `test3.txt` with content is `Hello world demo`
![avatar](/photos/git-under-the-hood/image-4.png)
Since the content of `test3.txt` is different from `test1.txt` and `test2.txt` so Git will store content of `test3.txt` in another blob with SHA-1 is `21e1c97cd06b685d3e19eec73a424af922cc01eb`
### TREE
Tree in Git is similar to a directory in operation system. Directory contains files and other directories, tree contains blob and other trees
![avatar](/photos/git-under-the-hood/image-5.png)(Update image from canva later)

Like I mentioned above, TREE also have its own SHA-1 
### COMMIT
Commit is a snapshot of our working directory. It points to a tree and contains some information such as: author, message, parent, etc. Also like BLOB and TREE, COMMIT have its own SHA-1. Let's back to our working directory. 

In the last examples, we have created 3 files `test1.txt`, `test2.txt` and `test3.txt`.
![avatar](/photos/git-under-the-hood/image-6.png)

Currently, those files still in staging area (where files are prepared for next commit but we will discover it later on). If we want to move those files to our repository, we should use `git commit`

![avatar](/photos/git-under-the-hood/image-7.png)
![avatar](/photos/git-under-the-hood/image-8.png)

You can see that I've already created a commit with the message is `Commit 1` and its SHA-1 is `725ce03ca79fff4314c183eac0110ea279344188`
## Git data model
Now we've already known Git's object types. Let's take some example to be more clearly
![avatar](/photos/git-under-the-hood/image-9.png)
Here is a graph to visualize relationship between those object types. In this section, I won't talk about HEAD, branch, remote, tag, only focus on blob, tree and commit.

For example, I have a directory like this
![avatar](/photos/git-under-the-hood/image-10.png)

Let's commit those files to see what happen
![avatar](/photos/git-under-the-hood/image-11.png)
Take a look at this graph, we can see that, Git structure our commit quite similar to working directory. The `tree(ef88c...)` node is a snapshot of our working directory and `commit(bd012...)` node will point to `tree(ef88c...)` node. Now let’s assume that I change the `index.js` file and commit again
![avatar](/photos/git-under-the-hood/image-12.png)
Since the content of `index.js` has  changed so its SHA-1 also been changed. That why git create another blob. One thing we should notice here is that why Git won't create another blob of `config.js` and `database.js`? 

It's quite easy to figure out from `index.js`. 

- `index.js` => Content changed => SHA-1 changed => New blob
- `config.js` => Content remain => SHA-1 remain => Old blob
- `database.js` => Content remain => SHA-1 remain => Old blob

Since the content of `config.js` and `database.js` didn't change, Git only need point the `tree(z123a...)` to `tree(012pq...)`.

How about delete `config.js`?
![avatar](/photos/git-under-the-hood/image-13.png)

Let's repeat above steps
- `index.js` => Content changed => SHA-1 changed => New blob
- `database.js` => Content remain => SHA-1 remain => Old blob
- `config.js` => Deleted => No blob

Notice that SHA-1 of `tree(012pq...)` made from SHA-1 of `blob(76mb6...)` and `blob(445zx...)` so any of those blob changed lead to change SHA-1 of `tree`. Since we deleted `config.js` so Git will create another tree that only need to point to `blob(445zx)`(database.js). The cool thing here is that even thought we deleted `config.js` but it's actually still being stored in Git database. When we back to `commit 1` or `commit 2`, we can still get `config.js`.
## `.git` folder
Have you ever tried to figure out what inside of it? If not, try with me.

![avatar](/photos/git-under-the-hood/image-14.png)

Here is what inside `.git` folder. We should care about 3 things:  `objects`, `refs` and `HEAD`.

### objects
As I mentioned early, objects just like a database of Git, it store blobs, trees and commits.

Let's create a commit



