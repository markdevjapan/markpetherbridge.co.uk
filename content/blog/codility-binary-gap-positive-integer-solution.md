---
title: "Codility Challenge: My Binary Gap Positive Integer Solution (Python)    "
date: 2020-03-23T10:35:16+09:00
draft: false
description: "My solution to the Codility Binary Gap (Positive Integer) task"
githubRepo: "http://github.com"
---

<p>For fun, education and self development I like to get involved with open source projects and coding challenges online. One of the most well known is online code test platforms is <a href="https://www.codility.com/" target="_blank">Codility</a>. </p>

<figure class="text-center"><img src="/img/codility/codility-logo.png" alt="Codility Logo" /></figure>

<p>Especially in Japan. Many of the top recruiters of developers, like Indeed, Google and Rakuten will use Codility and other testing platforms to test your ability to code efficient and scalable code.</p>

### The task

<p>* The following blockquote is directly taken from the <a href="https://app.codility.com/programmers/lessons/1-iterations/binary_gap/" target="_blank">Codility website</a>. Copyright remains exclusively to Codility. The use of the text here is for educational purposes only.</p>

<blockquote>
<p>A binary gap within a positive integer N is any maximal sequence of consecutive zeros that is surrounded by ones at both ends in the binary representation of N.</p>
<p>Examples:</p>
<ol>
<li>Number <span class="highlight">9</span> has binary representation <span class="highlight">1001</span> and contains a binary gap of length <span class="highlight">2</span>. </li>

<li>The number <span class="highlight">529</span> has binary representation <span class="highlight">1000010001</span> and contains two binary gaps: one of length <span class="highlight">4</span> and one of length <span class="highlight">3</span>. </li>

<li>The number <span class="highlight">20</span> has binary representation <span class="highlight">10100</span> and contains one binary gap of length <span class="highlight">1</span>.</li>

<li>The number <span class="highlight">15</span> has binary representation <span class="highlight">1111</span> and has <strong>no binary gaps</strong>.</li> 

<li>The number <span class="highlight">32</span> has binary representation <span class="highlight">100000</span> and has <strong>no binary gaps.</strong></li>
</ol>

<p>Write a function:</p>

<pre><code>def solution(N)</code></pre>

<p>that, given a positive integer <span class="highlight">N</span>, returns the length of its longest binary gap. The function should return <span class="highlight">0</span> if <span class="highlight">N</span> doesn't contain a binary gap.</p>

<p>For example, given <span class="highlight">N = 1041</span> the function should return <span class="highlight">5</span>, because <span class="highlight">N</span> has binary representation <span class="highlight">10000010001</span> and so its longest binary gap is of length <span class="highlight">5</span>. Given <span class="highlight">N = 32</span> the function should return <span class="highlight">0</span>, because <span class="highlight">N</span> has binary representation <span class="highlight">100000</span> and thus <strong>no binary gaps</strong>.</p>

<p>Write an efficient algorithm for the following assumptions:</p>

<p><span class="highlight">N</span> is an integer within the <span class="highlight">range [1..2,147,483,647]</span>.</p>
</blockquote>

### Smaller pieces are easier to digest

<p>Let's take the task and break it down into smaller pieces, I often find that by doing it this way helps me develop my applications as a well structured whole.</p>

<p><strong>What is it we actually want to achieve</strong></p>
<p>From the task, we are to return the maximal sequence of consecutive zeros from the result of an integer converted if there is at least a single <span class="highlight">0</span> after an instance of <span class="highlight">1</span> and before the next <span class="highlight">1</span>. Basically, we just need to count all the <span class="highlight">0</span> that fall between a <span class="highlight">1</span> and a <span class="highlight">1</span>. Each time this happens we say it is a new group, we then need to find the group with the highest amount of zeros in it.</p>
<p>For example, If we take the number <span class="highlight">29210</span> do some <a href="https://indepth.dev/the-simple-math-behind-decimal-binary-conversion-algorithms/" target="_blank">simple and quick maths</a> we get the binary representation of: </p>

<figure class="text-center"><img src="/img/codility/binary-gap/binary-rep-of-29210.png" alt="Binary Representation" /></figure>

<p>Next we need to create the groups of zeros that fall between a <span class="highlight">1</span> and <span class="highlight">1</span>. Using the above binary we can ascertain that there are 3 groups.</p>

<figure class="text-center"><img src="/img/codility/binary-gap/binary-def-groups.png" alt="Binary Representation Groups" /></figure>

<p>Finally, we need to return the length of the largest group. In this example the largest group is <span class="highlight">GROUP 2</span> and the length to be returned is: <span class="highlight">4</span></p>

### So how do we do that in Python?

<p><strong>1. Convert the Integer (N) into it's binary representation counterpart</strong></p>
<p>Python comes with some built in <a href="https://docs.python.org/3/library/functions.html" target="_blank">functions</a> and luckily one of these is <a href="https://docs.python.org/3/library/functions.html#bin" target="_blank">bin()</a>. This function takes an input of type <span class="highlight">Integer</span> and returns a binary string prefixed with <span class="highlight">0b</span>
<p>
<p>If we use an online Python compiler such as: <a href="https://repl.it/languages/python3" target="_blank">repl.it</a> we can check our code straight away.</p>
<p>Go to that website, and in the left panel where you type in your Python code, input:</p>
<script src="https://gist.github.com/markdevjapan/5ed821dedab2b6dac2133286c134baaa.js"></script>
<p>This will give us the result: <span class="highlight">0b1</span></p>
<p>We do not want the <span class="highlight">0b</span> at the beginning for our example so we can escape it using <span class="highlight">[2:]</span>, this is part of the built in <a href="https://docs.python.org/2/library/re.html" target="_blank">regular expressions</a>. By adding <span class="highlight">[2:]</span> to the end we are saying do not get the first two characters of the string. Now, The code should look like:</p>
<script src="https://gist.github.com/markdevjapan/ef7cfe939b9a40413b4084b6e12c9bba.js"></script>
<p>This will give us the result: <span class="highlight">1</span></p>

<p>Using the <a href="#smaller-pieces-are-easier-to-digest">above example</a> of converting text to binary, to achieve that here we would use something like:</p>
<script src="https://gist.github.com/markdevjapan/41dd88001f59c8e6fbb2786f90a9db32.js"></script>
<p>This will give us the result: <span class="highlight">111001000011010</span></p>


<p><strong>2. Work out what the groups are and return the largest</strong></p>
<p>Now we need to build the mechanism that will work out the groups of zeros as depicted <a href="#smaller-pieces-are-easier-to-digest">above</a>. Whenever you are faced with the task of defining a programs logic, it's best to use a <a href="https://en.wikibooks.org/wiki/Programming_Fundamentals/Flowcharts" target="_blank">flowchart</a>. A flowchart is a representation of the workflow of an application, an algorithm or process. </p>
<p>A flowchart will allow you to check your programs logic with different scenarios (test cases). To start with our flowchart we must first work out what parts of the mechanism we need to have. For this specific task, I would suggest that our code will do the following things:</p>
<ul>
<li>Loop through the returned string of integers (<span class="highlight">br</span>)</li>
<li>Check if following character after a 1 is a 0</li>
<li>Check if following character after a 0 is a 1</li>
<li>Start counting for a group</li>
<li>Stop counting for a group</li>
<li>Get the total for the current group</li>
<li>Check if current total is higher than any previous totals</li>
<li>Store highest group total</li>
</ul>

<p><strong>3. Putting it all together</strong></p>
Start looping through the string, The Codility test gives us our inputs as <span class="highlight">N</span> and suggests we pass them into our <span class="highlight">Solution()</span> function. like so:

<pre><code>def solution(N)
</code></pre>

<p>which means, we need to change our binary replace string above to: </p>

<pre><code>br = str(bin(N))[2:]
</code></pre>

<p>Let's also define some variables that we will use to; 1) Check if a new group count has started, 2) record the highest group so far and 3) Count the 0's in the group.</p>
<pre><code>br_group = False
br_highest = 0
bin_zero_counter = 0
</code></pre>

<p>Next, we'll loop through (using the <a href="https://wiki.python.org/moin/ForLoop" target="_blank">python ForLoop</a>) the returned binary converted string of characters:</p>

<pre><code>for char in br:</code></pre>

<p>This will assign each iteration character to that loop instance of <span class="highlight">char</span>. For example, using the returned string of <span class="highlight">110011</span>, each instant of the loop would change the value of char: </p>
<ol>
<li>char = 1</li>
<li>char = 1</li>
<li>char = 0</li>
<li>char = 0</li>
<li>char = 1</li>
<li>char = 1</li>
</ol>

<p>Each loop iteration allows us to run a block of code. Inside this we can use the python <a href="https://docs.python.org/3/tutorial/controlflow.html" target="_blank">if statement</a> to check the first part of our flowchart, if its true, run some code. However, we can also run code if it is not true:</p>

<pre><code># Check first condition:
if char == '1':
    # Run code needed if true
elif {condition} :
    # Run code needed if not true</code></pre>

<p>What we need here is to add an incremented value to the group counter as if the returned string is not a <span class="highlight">1</span> then it will be a <span class="highlight">0</span>. The above function then will change to:</p>

<pre><code># Check first condition:
if char == '1':
    # Run code needed if true
elif br_group:
    bin_zero_counter += 1</code></pre>

<p>In our 'if true' block, we start the group tracker by setting <span class="highlight">br_group</span> to <span class="highlight">true</span> and we set the <span class="highlight">bin_zero_counter</span> to <span class="highlight">0</span></p>

<pre><code>br_group = True
bin_zero_counter = 0</code></pre>

<p>We do need to check however if the current counter is higher than the highest recorded</p>

<pre><code>if br_highest < bin_zero_counter:
    br_highest = bin_zero_counter</code></pre>

And that sums up the breakdown of the code.

<p><strong>Completed code</strong></p>

<p>When we put it all together we get:</p>

<script src="https://gist.github.com/markdevjapan/b5109917b7c0cd807d2aa9d810e986b1.js"></script>

<p>And when we run it in the Codility validator, we get the result of:</p>
<figure class="text-center"><img src="/img/codility/binary-gap/binary-def-results.png" alt="Challenge Results 100%" /></figure>
<p class="text-center"><a href="https://app.codility.com/demo/results/trainingPZ7XR8-9WE/" target="_blank">View online result report</a></p>