---
title: "How to use (Handle/Consume) APIs in React Native"
date: 2020-06-03T10:49:04+09:00
draft: true
description: "How to use Javascript/ES6's .fetch() to consume APIs in React Native"
githubRepo: ""
---

link: https://medium.com/better-programming/handling-api-like-a-boss-in-react-native-364abd92dc3d


<p>Yesterday I was asked to refactor a React Native App that a Junior Developer on the team created. He used <strong class="highlight">XMLHttpRequest</strong>. When asked why, he said that he thought it was best practice because stack overflow had it upvoted and it was accepted as the answer.</p>
<blockquote>
*insert facepalm*
</blockquote>
<p>So I showed him a better way, and gave him the most valuable lesson any budding developer could need: Do not blindly copy and paste stack overflow code and no not assume that an accepted answer means best practice. AND, always check the published date. What could be best practice today may not be best practice tomorrow</p>
<p>Edit: After re-reading, I did not mean to come across as arrogant or rude, I was helping the JD and it was all very civil and critiqued in an educational way. I mean, he did cry though.. (jokes) </p>
<blockquote>
Are you wanting to get into coding? If so, check out this great article: <a href="https://tuttiq.me/where-do-i-start-on-learning-to-code/" target="_blank">Where Do I Start (learning to code)?</a> by <a href="https://www.linkedin.com/in/tuttiq/" target="_blank">Tutti Quintella</a> in Japan. 
</blockquote>

<h2>So long.. and a beloved farewell to XMLHttpRequest and a swanky swishy hello to fetch()</h2>
<p>If you have any sort of familiarity with Javascript, especially ES6 then you may have come across the term fetch. </p>