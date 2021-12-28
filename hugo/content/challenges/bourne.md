---
title: "XSS Game - Jason Bourne | PwnFunction"
header: "Jason Bourne"
challengeUrl: "https://sandbox.pwnfunction.com/challenges/bourne.html"
date: 2020-02-03T17:48:55+05:30
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
&#x3C;script&#x3E;
    /* Helpers */
    const bootstrapAlert = (msg, type) =&#x3E; {
        return (&#x60;&#x3C;div class=&#x22;alert alert-${type}&#x22; role=&#x22;alert&#x22;&#x3E;${DOMPurify.sanitize(msg)}&#x3C;/div&#x3E;&#x60;)
    }

    document.getAlert = () =&#x3E; document.getElementById(&#x27;alerts&#x27;);
&#x3C;/script&#x3E;

&#x3C;script&#x3E;
    /* Welcome */
    let name = (new URL(location).searchParams.get(&#x27;name&#x27;)) || &#x22;Pamela Landy&#x22;;
    document.write(
        bootstrapAlert(&#x60;&#x3C;b&#x3E;Operation Treadstone&#x3C;/b&#x3E;: Welcome &#x3C;u&#x3E;${name}&#x3C;/u&#x3E;.&#x60;, &#x27;info&#x27;)
    )
&#x3C;/script&#x3E;

&#x3C;!-- alerts --&#x3E;
&#x3C;div id=&#x22;alerts&#x22;&#x3E;&#x3C;/div&#x3E;

&#x3C;script&#x3E;
    /* Handle to &#x60;#alert&#x60; */
    let alerts = document.getAlert();

    /* Treadstone Credentials */
    let identification = Math.random().toString(36).slice(2);
    let code = Math.floor(Math.random() * 89999 + 10000);

    /* Default Credentials */
    DEFAULTS = {};
    DEFAULTS[identification] = code;
&#x3C;/script&#x3E;

&#x3C;script&#x3E;
    /* Optional Comment */
    if (location.hash) {
        let comment = document.createComment(decodeURI(location.hash).slice(1));
        document.querySelector(&#x27;#alerts&#x27;).appendChild(comment);
    }
&#x3C;/script&#x3E;

&#x3C;script&#x3E;
    /* Use &#x60;DEFAULTS&#x60; to init &#x60;SECRETS&#x60; */
    SECRETS = DEFAULTS

    /* Increment the &#x60;code&#x60; before the check */
    let secretKey = new URL(location).searchParams.get(&#x27;key&#x27;) || &#x22;TREADSTONE_WEBB&#x22;;
    SECRETS[secretKey] += 1;

    /* Authorization Check */
    if (SECRETS[secretKey] === SECRETS[identification]) {
        confirm(&#x60;Jesus Christ, it&#x27;s Jason Bourne!&#x60;)
    } else {
        confirm(&#x60;You ain&#x27;t David Webb!&#x60;)
    }
&#x3C;/script&#x3E;
</code>{{< /code >}}

<!-- Solution Modal -->
{{< solution >}}
<p>Solution is...</p>
<pre class="solution-code-block"><code class="language-markup">?name=&#x3C;img name=getAlert&#x3E;&#x3C;form id=alerts name=DEFAULTS&#x3E;&#x26;key=innerHTML#--&#x3E;&#x3C;img src onerror=alert(1337)&#x3E;</code></pre>
<p>The idea is to inject <code>--&#x3E;&#x3C;img src onerror=alert(0)&#x3E;</code> via the <code>location.hash</code> and trigger the HTML mutation via making changes using <code>innerHTML</code>. A perfect candidate is <code>SECRETS[secretKey]</code> because we control the property <code>secretKey</code>. If we can set the <code>SECRETS</code> variable to the <b>div</b> that holds our comment and set the <code>secretKey</code> to <code>innerHTML</code> then it's game over because it triggers the mutation. A quick scenario might be something like the following.</p>

<pre class="solution-code-block"><code class="language-js">x = document.createComment(&#x22;--&#x3E;&#x3C;img src onerror=alert(1)&#x3E;&#x22;)
div.appendChild(x)
div.innerHTML += &#x22;blah&#x22; // triggers mutation</code></pre>

<p>Our comments land inside the <b>div</b> which has the ID <code>alerts</code> and we need to set <code>SECRETS</code> variable to this <b>div</b>. But to set the <code>SECRETS</code> variable to the <b>div</b> we should first undefine the variable <code>DEFAULTS</code> so that we can use <a target="_blank" class="function" href="https://youtu.be/2up8J9dErHI?t=797">DOM Clobbering</a> to redefine it to a <b>div</b> Element. Why <code>DEFAULTS</code>? because in the following script block, you'll see that <code>SECRETS</code> becomes <code>DEFAULTS</code>.</p>

<pre class="solution-code-block"><code class="language-js">/* Use &#x60;DEFAULTS&#x60; to init &#x60;SECRETS&#x60; */
SECRETS = DEFAULTS</code></pre>

<p>Now lets first undefine the <code>DEFAULTS</code> variable. One way is to cause an error in the script block that creates this variable so that the subsequent code wouldn't run and hence <code>DEFAULTS</code> variable wouldn't be created in the first place.</p>

<p>We'll cause an error in the following line of code.</p>
<pre class="solution-code-block"><code class="language-js">/* Handle to &#x60;#alert&#x60; */
let alerts = document.getAlert();</code></pre>

<p>The idea here is to create an element with the <b>name</b> <code>getAlert</code> which will clobber the <code>document</code> property, so <code>document.getAlert</code> will be no longer a function but an HTML Element. When it's called, it'll thrown an error because <code>document.getAlert</code> is no longer a function, which will stop executing javascript from running the subsequent lines of code which ultimately means that the <code>DEFAULTS</code> variable will never be created.</p>

<p>To clobber <code>document.getAlert</code> we'll use <code>img</code> because it can clobber using the <code>name</code> attribute.</p>

<pre class="solution-code-block"><code class="language-markup">&#x3C;img name=getAlert&#x3E;</code></pre>

<p>Earlier I told you that we need to set the <code>DEFAULTS</code> variable to the <b>div</b> element which has the ID set to <code>alerts</code>, well I lied. We can simply create a new Element with the ID set to <code>alerts</code> and it's <b>name</b> to <code>DEFAULTS</code>  so that it clobbers the <code>DEFAULTS</code> variable. We can simply use <code>form</code> because it can have the ID set to <code>alerts</code> but also clobber via the <code>name</code> attribute which will be set to <code>DEFAULTS</code>.</p>

<pre class="solution-code-block"><code class="language-markup">&#x3C;form id=alerts name=DEFAULTS&#x3E;</code></pre>

<p>Now that we control the <code>SECRETS</code> variable and also <code>secretKey</code>, all there's left is to set the <code>secretKey</code> to <code>innerHTML</code> which triggers the mutation.</p>

<pre class="solution-code-block"><code class="language-js">SECRETS[secretKey] += 1;
// FORMElement['innerHTML'] += 1;</code></pre>
<br>
<p><u><b>Steps</b></u><br/>
<ul>
    <li>Undefine <code>DEFAULTS</code> by clobbering <code>document.getAlert</code>.</li>
    <li>Redefined <code>DEFAULTS</code> to a Form Element with the ID <code>alerts</code> so that <code>SECRETS</code> will be this Form Element.</li>
    <li>Set <code>location.hash</code> to <code>--&#x3E;&#x3C;img src onerror=alert(0)&#x3E;</code> which will land inside the Form Element because it's ID is <code>alerts</code>.</li>
    <li>Set <code>secretKey</code> to <code>innerHTML</code> so that when <code>SECRETS[secretKey] += 1</code> runs, this will trigger the mutation.</li>
</ul>

Ergo XSS.
</p>
{{< /solution >}}

<!-- Solvers -->
{{< solvers >}}
<tr>
    <th scope="row">1</th>
    <td><a href="https://twitter.com/@SecurityMB" target="_blank" class="function">Michał Bentkowski</a>
    </td>
    <td>Unintended(fixed) & Intended</td>
</tr>
<tr>
    <th scope="row">2</th>
    <td><a href="https://twitter.com/@terjanq" target="_blank" class="function">terjanq</a>
    </td>
    <td>Unintended(fixed) & Intended</td>
</tr>
<tr>
    <th scope="row">3</th>
    <td><a href="https://twitter.com/@Abdulahhusam" target="_blank" class="function">Abdullah Hussam</a>
    </td>
    <td>Unintended(fixed) & Intended</td>
</tr>
<tr>
    <th scope="row">4</th>
    <td><a href="https://twitter.com/@lbherrera_" target="_blank" class="function">Luan Herrera</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">5</th>
    <td><a href="https://twitter.com/@insertScript" target="_blank" class="function">alex</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">6</th>
    <td><a href="https://twitter.com/@krial057" target="_blank" class="function">Alain</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">7</th>
    <td><a href="https://twitter.com/@RootEval" target="_blank" class="function">RootEval</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">8</th>
    <td><a href="https://twitter.com/@kuntekinte" target="_blank" class="function">kunte_</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">9</th>
    <td><a href="https://twitter.com/@yardnsm" target="_blank" class="function">Yarden Sod-Moriah</a>
    </td>
    <td>Intended</td>
</tr>
{{< /solvers >}}