---
layout: post
title: UX Pain \#1 - links not being links
---

<p>It's not a new idea and... It's kinda obvious but there are a lot of buttons or "links" on the web which are not links. But who has JavaScript disabled you ask! Well, there are other reasons...</p>

<h2>Google, you are doing it wrong.</h2>

<p>One of the most embarrassing displays of this mistake happened to Google Chrome download page on 4th February 2013 when the <a href="http://thenextweb.com/google/2013/02/04/weird-right-now-no-one-can-download-and-install-google-chrome-from-the-official-site/">Chrome download link just didn't work</a>. And if nobody can download your browser from official site, how much money and influence you lose? The problem was in JavaScript which is handling their download button and still they haven't changed anything at all on <a href="https://www.google.com/intl/en/chrome/browser/">Chrome's download site</a>. Try downloading Chrome without JavaScript.</p>

<p>Oops! The <code>javascript:void(0)</code> doesn't work? Maybe try <a href="http://www.opera.com/download">Opera</a> or <a href="http://www.mozilla.org/en-US/firefox/new/">Firefox</a>, they have learned the lesson...</p>

<h2>Please TED, please...</h2>

<p>Of course I don't download Chrome every day. I just wanted to prove my point, pure JavaScript links are bad bad decision. What bothers me almost every day is approach of <a href="http://www.ted.com">ted.com</a>. I wrote them request to convert their lovely <code>&lt;div&gt;</code> buttons to proper links because when you want to watch a talk in new tab...  </p>

<p>Middle click? Oops! Right click and "Open in new tab"? Oops! Don't have JavaScript? Just leave...</p>

<p>It's so easy to do it right yet many people just don't bother. I would love to hear why they made this decision. Meanwhile I can only duplicate every TED's tab and cry in the shower (and on my blog)...</p>

<p>What other sites with broken "links" have you come across? Did you report the problem?</p>	
