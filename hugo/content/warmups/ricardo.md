---
title: "XSS Game - Ricardo Milos | PwnFunction"
header: "Ricardo Milos"
challengeUrl: "https://sandbox.pwnfunction.com/warmups/ricardo.html"
date: 2020-01-19T18:16:15+05:30
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
&#x3C;form id=&#x22;ricardo&#x22; method=&#x22;GET&#x22;&#x3E;
    &#x3C;input name=&#x22;milos&#x22; type=&#x22;text&#x22; class=&#x22;form-control&#x22; placeholder=&#x22;True&#x22; value=&#x22;True&#x22;&#x3E;
&#x3C;/form&#x3E;
&#x3C;script&#x3E;
    ricardo.action = (new URL(location).searchParams.get(&#x27;ricardo&#x27;) || &#x27;#&#x27;)
    setTimeout(_ =&#x3E; {
        ricardo.submit()
    }, 2000)
&#x3C;/script&#x3E;</code>{{< /code >}}

{{< solution >}}
<p><code>ricardo.action</code> is set to a user controlled input through a GET parameter
<code>ricardo</code> and the form is auto submitted,
ergo a Javascript URI would do it.</p>

<pre class="solution-code-block"><code class="language-js">javascript:alert(1337)</code></pre>
{{< /solution >}}