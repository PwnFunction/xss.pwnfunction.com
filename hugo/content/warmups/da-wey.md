---
title: "XSS Game - Ugandan Knuckles | PwnFunction"
header: "Ugandan Knuckles"
challengeUrl: "https://sandbox.pwnfunction.com/warmups/da-wey.html"
date: 2020-01-19T18:11:02+05:30
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
&#x3C;div id=&#x22;uganda&#x22;&#x3E;&#x3C;/div&#x3E;
&#x3C;script&#x3E;
    let wey = (new URL(location).searchParams.get(&#x27;wey&#x27;) || &#x22;do you know da wey?&#x22;);
    wey = wey.replace(/[&#x3C;&#x3E;]/g, &#x27;&#x27;)
    uganda.innerHTML = &#x60;&#x3C;input type=&#x22;text&#x22; placeholder=&#x22;${wey}&#x22; class=&#x22;form-control&#x22;&#x3E;&#x60;
&#x3C;/script&#x3E;</code>{{< /code >}}

{{< solution >}}
<p>The GET parameter <code>wey</code> becomes a part of the <code>uganda.innerHTML</code>, but
    before that it filters a bunch of characters out.
    <pre class="solution-code-block"><code class="language-js">wey = wey.replace(/[<>]/g, '')</code></pre>
    The lesser-than(&lt;) & greater-than(&gt;) characters are removed, this means we
    can't
    create
    new tags. But quotes are not removed, so we can break out of the <code>placeholder</code>
    and inject some attributes. Since we can't have user interactions, we can use
    <code>onfocus</code> event handler and then autofocus the input to trigger your code.
</p>

<pre class="solution-code-block"><code class="language-js">"onfocus=alert(1337) autofocus="</code></pre>
{{< /solution >}}