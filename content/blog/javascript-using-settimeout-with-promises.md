---
title: "Using Javascript Settimeout() With Promises"
date: 2019-05-23T11:48:29+09:00
draft: true
description: "A commonly googled problem that people seem to encounter. I thought I'd share my solution"
aliases: 
  - /blog/javascript/javascript-using-settimeout-with-promises/
---

<figure class="text-center"><img src="/img/javascript-using-settimeout-with-promises/javascript-promise-chain-diagram.png" alt="Javascript Promise Example" /></figure>

<em>This article is intended to be a short and sweet.</em> 

At work and for one of our projects we use <a href="https://aws.amazon.com/polly/" rel="noopener" target="_blank">Amazon Polly</a> and I was assigned a <a href="https://www.atlassian.com/software/jira" rel="noopener" target="_blank">JIRA</a> ticket today to handle an error with the call to the API. At some point our application has grown bigger than originally expected and we are now getting the following error: <span class="highlight">ThrottlingException: Rate exceeded</span>. 

<blockquote>I feel that I must also point out that this is a great example of bad planning and as a developer you should always plan and build your applications with scalability in mind. "should" being the verb that strikes fear in developers.</blockquote>

Anyway, I thought great! This is just a quick fix that will be resolved using <strong class="highlight">setTimeout()</strong>

After some initial digging, I found the function that made the call to AWS Polly:


<pre><code>function createAudioFiles(data, outputDir) {
    return new Promise((resolve, reject) => {
      let successessfullyCompletedAmount = 0;

      for ({ audioText, filename } of data) {

          createAudio(audioText, filename, outputDir)

          .then(({ status, message }) => {
            if (status == "success") {
              successessfullyCompletedAmount++;
            }

            // if all audio files have been created
            if (successessfullyCompletedAmount == data.length) {
              resolve({
                status: 'success',
                message: "successfully created audioFiles"
              })
            }

          })
      }
    });
  }</code></pre>

In the above function there is a for loop which contains a call to another function:

<pre><code>createAudio(audioText, filename, outputDir);</code></pre>

The only thing that is in that function is the <a href="https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Polly.html" rel="noopener" target="_blank">AWS Polly SDK</a> code so I will not list it all here. I wrapped that function call in a <a href="https://javascript.info/settimeout-setinterval" rel="noopener" target="_blank">setTimeout()</a> method with a 3 second timeout:

<pre><code>setTimeout(function() {
	createAudio(audioText, filename, outputDir);
}, 3000);</code></pre>

Job done, or so I thought. When I ran the code in a Terminal window I got the same Throttle Limit Exceeded message. 

After some further investigation I realised by oversight and the issue was even though we were adding the timeout, it only delayed each one by 5 seconds and then added the next one straight behind it. So after 5 seconds, they all ran in sequence again.

[chained photo here]

What I needed to do was break up the block of data that is been sent and then to send those in batches. When and only when the previous block was completed. We can check when all the items of the data have been resolved by using the <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all" rel="noopener" target="_blank">Promise.all()</a> method. 

<blockquote>
The Promise.all() method returns a single Promise that resolves when all of the promises passed as an iterable have resolved or when the iterable contains no promises. It rejects with the reason of the first promise that rejects.</blockquote>


<h4>Refactoring the function</h4>

Inside my current promise <span class="highlight">return new Promise((resolve, reject) => { ... }</span> I created another function and added a function call:
<pre><code>let fnGet = (arr, size) => {
	// Functionality here.
}
fnGet(data, 30);</code></pre>

<span class="highlight">data (arr)</span> is what is passed into the function wrapping this one and is an array of text to convert and output filenames. <span class="highlight">30 (size)</span> is is the amount of requests we want to send each time. At the moment, the data contains around 500 array objects and it is this what is causing the issues. I just put 30 as a random low number but you might want to check what the rate limits are for whatever service you are using this for.

Ok, now we want to "chop" up the array so that we have smaller sizes. For this I am going to use <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice" rel="noopener" target="_blank">array slicing</a> to take the required amount (30) off the array.

<blockquote>The slice() method returns a shallow copy of a portion of an array into a new array object selected from begin to end (end not included). The original array will not be modified.</blockquote>

<pre><code>let remainder = [...(arr.slice(size))]</code></pre>

This line of code does two things: 1) It makes takes 30 objects from the <span class="highlight">arr</span> array and reassigns <span class="highlight">arr</span> with those 30 items and 2) whatever is left is assigned to the <span class="highlight">remainder</span> variable.

I then created an empty array so I can add the generated promises:

<pre><code>promises = [];</code></pre>

What I now needed was to loop through the 30 (the size variable) items that are left in the array and send them to the API function and return a promise. I created a <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration" rel="noopener" target="_blank">for loop</a> which would iterate through the array, send the data to the function and store the returned promise into the <span class="highlight">promises</span> array using the <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push" rel="noopener" target="_blank">array.push</a> method:

<pre><code>for(let i = 0 ; i < size ; i++) {
    promises.push(createAudio(arr[i].audioText, arr[i].filename, outputDir)
        .then(({ status, message }) => {
            if (status == "success") {
                successessfullyCompletedAmount++;
            }
        })
    )
}</code></pre>

The <span class="highlight">successessfullyCompletedAmount++</span> counter was already part of the code and I left that in just incase this is used somewhere else that I am not sure of yet.

Now I can implement the <span class="highlight">Promise.all</span> method as mentioned above which check if all the promises resolve from the promises array. I can then chain functionality onto that method using the <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then" rel="noopener" target="_blank">.then()</a> method which will trigger when it returns true. 

<blockquote>The then() method returns a Promise. It takes up to two arguments: callback functions for the success and failure cases of the Promise.</blockquote>

Below is the basic structure of the <span class="highlight">Promise.all()</span>.

<pre><code>Promise.all(promises)
.then( () => {
    // Do something.
}, (err) => {
    reject({
        status: 'failure',
        message: "An error occurred getting the audio"
    })
})
</code></pre>

The section of code that will go into the <span class="highlight">then()</span> part will be where we check if we have anything else to process or if we can resolve it. 

Ok, so let's check if there is anything else in the remainder to process and if not, then we can complete. We do this with a simple <span class="highlight"if</span> condition statement:

<pre><code>if(remainder.length === 0) {
    resolve({
        status: 'success',
        message: "successfully created the audio Files"
    });
}
</code></pre>

Now let's turn that if statement into an <span class="highlight">if-else</span> one because if there is something left to work on then we need to make sure we process it and add it to the promises array and remove it from the remainder array. This is also where we will be using the <span class="highlight">setTimeout()</span> method that I mi-used earlier: 

<pre><code>if(remainder.length === 0) {
    resolve({
        status: 'success',
        message: "successfully created audio Files"
    });
} else {
    setTimeout( () => {
        fnGet(remainder, size > remainder.length ? remainder.length : size);
    }, 1500)
}</code></pre>

And there you have it. A short and sweet technique to split apart a call to an API that returns a promise into smaller calls each with their own promises, we then wait for all those sub promises to be completed and then we can resolve the main promise returned.