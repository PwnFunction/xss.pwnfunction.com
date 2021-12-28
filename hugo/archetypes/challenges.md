---
title: "XSS Game - Challenge | PwnFunction"
header: "Challenge"
challengeUrl: "https://sandbox.pwnfunction.com/challenges/challenge-slug.html"
live: "true"
date: {{ .Date }}
draft: true
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
<li>Solutions on ...</li>
<!-- <li>Unintended solution? DM me <a target="_blank"
                            href="https://twitter.com/messages/compose?recipient_id=1084132461133451264"
                            class="function">@PwnFunction</a>.
                    </li> -->
{{</ rules >}}

<!-- Challenge Code -->
{{< code >}}<code class="language-markup">&#x3C;!-- Challenge --&#x3E;
Challenge Code
</code>{{< /code >}}

<!-- Solution Modal -->
{{< solution >}}
    Solution
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

{{< solved >}}
<a href="https://twitter.com/messages/compose?recipient_id=1084132461133451264"
                    target="_blank"><span class="function">@PwnFunction</span></a>.
{{< /solved >}}