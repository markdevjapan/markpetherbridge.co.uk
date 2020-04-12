---
title: "How to Install Elusive Sublime Babel VS Code plugin (3 steps)"
date: 2020-04-12T16:13:03+09:00
draft: false
description: "Do not fret. You can still install the Sublime Babel Plugin. "
githubRepo: ""
---

<blockquote>This is a quick article and more for a reference for myself really.</blockquote>

I recently have been getting to grips with Reach Native as I am working on a new app project for a client. Like most "new to a framework" developers I first typed into Google "How to build a React Native App". Favoring videos I switched to Youtube and found a Net ninja special course. Yay.

However, like most media on the net, especially videos. They often out-date very quickly, recommended packages and software is no longer available and updates (especially JS frameworks) are so often that you really can fall behind quickly. 

This was the case with this video. It is recommended that you install and use a VS Code plugin called <strong>Sublime Babel</strong> which is no longer available on the Visual Studio Code plugin directory. But do not Fret, there is a way! 

# Step 1: Download from the Repo 

Head over the the plug-in developers repo on <a href="https://github.com/joshpeng/Sublime-Babel-VSCode" target="_blank">Github</a> and download the repo as a zip. Dont forget to unzip it. 

# Step 2: Move to extension folder

Now, I use a mac, so this is for mac only. You will need to find the location of your extensions folder and the correct terminal command for that on other operating systems. 

In terminal, type in: 

<pre><code>$ cd .vscode/extensions</code></pre>

and then type in:

<pre><code>$ mv ~/Downloads/Sublime-Babel-VSCode-master .</code></pre>

# Step 3: Restart Visual Studio

Restart your IDE. You should now see the Sublime text plugin installed and ready to use. 


Its that simple, Enjoy! 

