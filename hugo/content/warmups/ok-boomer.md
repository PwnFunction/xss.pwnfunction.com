---
title: "XSS Game - Ok, Boomer | PwnFunction"
header: "Ok, Boomer"
challengeUrl: "https://sandbox.pwnfunction.com/warmups/ok-boomer.html"
date: 2020-01-19T18:35:26+05:30
draft: false
type: warmup
difficulty: Easy
---

{{< rules >}}
<li>Difficulty is <b>Easy</b>.</li>
<li>Pop an <code>alert(1337)</code> on <code>sandbox.pwnfunction.com</code>.</li>
<li>No user interaction.</li>
<li>Cannot use <code>https://sandbox.pwnfunction.com/?html=&js=&css=</code>.</li>
<li>Tested on <b>Chrome</b>.</li>
{{</ rules >}}

{{< code >}}<code class="language-markup">&#x3C;!-- Challenge --&#x3E;
&#x3C;h2 id=&#x22;boomer&#x22;&#x3E;Ok, Boomer.&#x3C;/h2&#x3E;
&#x3C;script&#x3E;
    boomer.innerHTML = DOMPurify.sanitize(new URL(location).searchParams.get(&#x27;boomer&#x27;) || &#x22;Ok, Boomer&#x22;)
    setTimeout(ok, 2000)
&#x3C;/script&#x3E;</code>{{< /code >}}

{{< solution >}}
<p>The GET parameter <code>boomer</code> is set as <code>boomer.innerHTML</code>, but the issue is
that the website uses <b>DOMPurify</b>, which sanitizes any Javascript and only leaves behind
script-less parts. The very next statement is a sink, <code>setTimeout</code> executes code
after a timeout specified in milliseconds. <code>setTimeout</code> can take a <i>function</i> or
a <i>string</i> as it's argument and execute it. Here the <code>ok</code> variable is being
executed, but it doesn't exist. Now they question is, can we define that <code>ok</code>
variable ourselves?<br><br> Yes, we can. <br> With <a
    href="http://www.thespanner.co.uk/2013/05/16/dom-clobbering/" target="_blank"><b>DOM
        Clobbering</b></a>. By
injecting HTML elements into the DOM, we can create Javascript variables. So in our case we need
to create the variable <code>ok</code>. To do so, we'll create an <a
    href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a" target="_blank"><b>Anchor
        Tag</b></a>
because if we create an anchor tag with the <code>id</code> set to <code>ok</code>, then the
browser automatically creates a variable named <code>ok</code> in Javascript. There's another
reason why we choose the anchor tag and it's because, when <code>toString()</code> is called on
an anchor tag, it returns the <code>href</code> property of that anchor tag object. This is
useful because not only we can control the variable creation, but also it's string value. So in
our case, when <code>setTimeout</code> tries to call the provided function <code>ok</code> as
the argument, it realises that it's not a function, so it calls the <code>toString()</code> on
it which returns the <code>href</code> property which gets executed.
<pre
    class="solution-code-block"><code class="language-markup">&lt;a id=ok href=tel:alert(1337)&gt;</code></pre>
Things to note:
<ul>
    <li><code>href</code> cannot be any arbitrary string, it has to follow
        <code>protocol:host</code> format, if the string doesn't follow the format, it's value
        will be <code>BaseURL/yourString</code>.</li>
    <li><code>tel:alert(1337)</code> is also a valid Javascript, because it follows
        <code>label:code</code> syntax.</li>
    <li><code>tel</code> is used because it's <a
            href="https://github.com/cure53/DOMPurify/blob/master/src/regexp.js#L12"
            target="_blank"><b>whitelisted</b></a>
        as one of the safe protocols to be allowed
        by DOMPurify.</li>
</ul>
{{< /solution >}}