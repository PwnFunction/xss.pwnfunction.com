---
title: "XSS Game - Me and the Bois | PwnFunction"
header: "Me and the Bois"
challengeUrl: "https://sandbox.pwnfunction.com/challenges/me-and-the-bois.html"
date: 2020-03-14T01:53:46+05:30
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
&#x3C;div id=&#x22;bois&#x22;&#x3E;
&#x3C;/div&#x3E;

&#x3C;script&#x3E;
    /* Variables */
    let safeTags = [&#x27;a&#x27;, &#x27;area&#x27;, &#x27;b&#x27;, &#x27;br&#x27;, &#x27;col&#x27;, &#x27;code&#x27;, &#x27;div&#x27;, &#x27;em&#x27;, &#x27;hr&#x27;, &#x27;h1&#x27;, &#x27;h2&#x27;, &#x27;h3&#x27;, &#x27;h4&#x27;, &#x27;h5&#x27;, &#x27;h6&#x27;, &#x27;i&#x27;, &#x27;iframe&#x27;, &#x27;img&#x27;, &#x27;li&#x27;, &#x27;ol&#x27;, &#x27;p&#x27;, &#x27;pre&#x27;, &#x27;s&#x27;, &#x27;small&#x27;, &#x27;span&#x27;, &#x27;sub&#x27;, &#x27;sup&#x27;, &#x27;strong&#x27;, &#x27;u&#x27;, &#x27;ul&#x27;]
    let forbiddenAttrs = [&#x27;style&#x27;, &#x27;srcdoc&#x27;]
    let cssSafe = /[^a-zA-Z0-9\s\-\,\:\_\(\)\{\}\&#x22;\&#x27;\.\#\;\%]/g

    /* Inputs */
    let boi = &#x60;&#x3C;h1&#x3E;${(new URL(location).searchParams.get(&#x27;boi&#x27;)) || &#x27;Neo&#x27;}&#x3C;/h1&#x3E;&#x60;
    let clean = DOMPurify.sanitize(boi, { ALLOWED_TAGS: safeTags, FORBID_ATTR: forbiddenAttrs })
    let bois = document.getElementById(&#x27;bois&#x27;)
    bois.innerHTML += clean;

    /* Custom Style JSON */
    let custom = (new URL(location).searchParams.get(&#x27;custom&#x27;)) || &#x22;&#x22;
    custom = custom.replace(cssSafe, &#x27;&#x27;)
    if (custom) {
        customStyles = JSON.parse(custom)
        let comment = document.createComment(customStyles)
        bois.appendChild(comment)
    }

    /* Configuration */
    window.CONFIG = {
        color: &#x22;lime&#x22;,
        backgroundColor: &#x22;#000&#x22;
    }
&#x3C;/script&#x3E;

&#x3C;script&#x3E;
    /* Generic Style Setter */
    function styleSetter(styles, execStr) {
        for (var style in styles) {
            if (styles.hasOwnProperty(style)) {
                eval(execStr)
            }
        }
    }

    /* Default Styles */
    window.DEFAULTS = {
        borderRadius: &#x22;5px&#x22;,
        fontFamily: &#x22;Space Mono&#x22;,
        fontWeight: &#x22;700&#x22;,
        letterSpacing: &#x22;4px&#x22;,
        padding: &#x22;20px&#x22;,
        textAlign: &#x22;center&#x22;,
        width: &#x22;500px&#x22;
    }
    styleSetter(DEFAULTS, &#x60;CONFIG[style] = styles[style]&#x60;)

    /* Custom Styles */
    if (window.customStyles) {
        styleSetter(customStyles, &#x60;CONFIG[style] = customStyles[style]&#x60;)
    }

    /* Stylise! */
    styleSetter(CONFIG, &#x60;bois.firstElementChild.style[style] = CONFIG[style]&#x60;)
&#x3C;/script&#x3E;
</code>{{< /code >}}

<!-- Solvers -->
{{< solvers >}}
<tr>
    <th scope="row">1</th>
    <td><a href="https://twitter.com/@Black2Fan" target="_blank"
            class="function">Sergey Bobrov</a>
    </td>
    <td>Unintended(fixed) & Intended</td>
</tr>
<tr>
    <th scope="row">2</th>
    <td><a href="https://twitter.com/@insertScript" target="_blank"
            class="function">alex</a>
    </td>
    <td>Unintended(fixed) & Intended</td>
</tr>
<tr>
    <th scope="row">3</th>
    <td><a href="https://twitter.com/@RootEval" target="_blank"
            class="function">RootEval</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">4</th>
    <td><a href="https://twitter.com/@SecurityMB" target="_blank"
            class="function">Michał Bentkowski</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">5</th>
    <td><a href="https://twitter.com/@IvarsVids" target="_blank"
            class="function">Ivars Vids</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">6</th>
    <td><a href="https://twitter.com/@BitK_" target="_blank"
            class="function">BitK</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">7</th>
    <td><a href="https://twitter.com/@Zeev42746664" target="_blank"
            class="function">Zeev</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">8</th>
    <td><a href="https://twitter.com/@StructHack" target="_blank"
            class="function">Dipendra Shrestha</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">9</th>
    <td><a href="https://twitter.com/@shafigullin" target="_blank"
            class="function">Roman</a>
    </td>
    <td>Intended</td>
</tr>
{{< /solvers >}}

{{< solution >}}
<p>Solution is ...</p>
<pre class="solution-code-block"><code class="language-markup">?boi=&#x3C;iframe id=CONFIG src=/&#x3E;&#x26;custom={&#x22;toString&#x22;:0,&#x22;src&#x22;:&#x22;javascript:alert(1337)&#x22;}
</code></pre>

The first thing to notice is that <code>iframe</code> tag is in the whitelisted tags. Also there's HTML injection via the <code>boi</code> <b>GET</b> parameter.
<pre class="solution-code-block"><code class="language-js">let safeTags = [..., &#x27;iframe&#x27;, ...]
/* ... */
// Sanitized with DOMPurify
bois.innerHTML += clean;</code></pre>

<p>There's also a feature to add some CSS styles to your HTML element via the <code>custom</code> <b>GET</b> paramater. The <code>custom</code> parameter takes input in JSON. </p>

<p>Now the idea is to DOM Clobber the <code>CONFIG</code> variable and set it to a <code>window</code> object. This is possible because <code>iframe</code> is in the whitelist and when clobbered via the <code>name</code> property, the value of the variable will be set to the <code>window</code> object of that iframe. Once clobbered, we can set the <code>src</code> property to any path on the website and then immediately we can change it to <code>javascript:alert(0)</code>, which will run the Javascript in the current website's context, hence XSS.</p>

<p>First off, to clobber the <code>CONFIG</code> variable, we need to first undefine it, since it already exists. To do this we can cause an error in the first script block. There are 2 ways to cause an error and undefine the <code>CONFIG</code> variable, but only one can lead to XSS. Basically, we set the <code>toString</code> of the <code>customStyles</code> to a value which can't be called, because <code>toString</code> is supposed to be a function that returns the string representation of that object.</p>

<p>Here, we have an indirect call to the <code>toString()</code> function, this happens when the code tries to create a comment from the object. Since comments are pretty much TextNodes, the object first has to be converted to a string, which is done via the <code>toString()</code> function.</p>
<pre class="solution-code-block"><code class="language-js">let comment = document.createComment(customStyles)
bois.appendChild(comment)</code></pre>

<p>Hence, we can set the <code>toString</code> to a number.
<pre class="solution-code-block"><code class="language-js">custom={&#x22;toString&#x22;:0}</code></pre>
<p>When called would trigger an error and the code below it in the same script block won't run, hence <code>WINDOW.CONFIG</code> will never be created. This is possible because the <code>CONFIG</code> is created at the runtime.</p>

<p>Now that we've undefined the <code>CONFIG</code> vairable, we can clobber it.</p>
<pre class="solution-code-block"><code class="language-markup">&#x3C;iframe name=CONFIG src=/&#x3E;</code></pre>
So now the <code>CONFIG</code> vairable, is a window object and we fully control it. How? check this code.

<pre class="solution-code-block"><code class="language-js">if (window.customStyles) {
    styleSetter(customStyles, &#x60;CONFIG[style] = customStyles[style]&#x60;)
}</code></pre>

<code>CONFIG[style]</code> is set to <code>customStyles[style]</code> and we control <code>customStyles</code> via the <b>GET</b> parameter <code>custom</code>. So it's simply just a matter of setting the <code>src</code> property to <code>javascript:alert(1337)</code>.

<pre class="solution-code-block"><code class="language-markup">?boi=&#x3C;iframe id=CONFIG src=/&#x3E;&#x26;custom={&#x22;toString&#x22;:0,&#x22;src&#x22;:&#x22;javascript:alert(1337)&#x22;}
</code></pre>

<p>The unintended solutions were similar, but instead of setting <code>src</code> to <code>javascript:alert(1337)</code>, the <code>innerHTMl</code> was used to set the HTML of a <code>div</code> element. So I added some regex in-place (<code>cssSafe</code>).</p>
{{< /solution >}}