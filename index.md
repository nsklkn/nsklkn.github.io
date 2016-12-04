---
layout: default
---
<header class="masthead">
  <h1 class="masthead-title">
    <a href="{{ site.baseurl }}/">{{ site.name }}<span>&#39;s blog</span></a>
  </h1>
  <nav class="masthead-nav">
    {% for nav in site.nav %}
    <a href="{{ nav.href }}">{{ nav.name }}</a>
    {% endfor %}
  </nav>
</header>
<div class="content list">
{% if site.posts.size == 0 %}
  <h2>No post found</h2>
{% else %}
{% for post in site.posts %}
  <div class="list-item">
    <h2 class="list-post-title">
      <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
    </h2>
    <div class="list-post-date">
      <time>{{ post.date | date_to_string }}</time>
    </div>
  </div>
{% endfor %}
{% endif %}
<!-- Yandex.Metrika counter --> <script type="text/javascript"> (function (d, w, c) { (w[c] = w[c] || []).push(function() { try { w.yaCounter41352714 = new Ya.Metrika({ id:41352714, clickmap:true, trackLinks:true, accurateTrackBounce:true }); } catch(e) { } }); var n = d.getElementsByTagName("script")[0], s = d.createElement("script"), f = function () { n.parentNode.insertBefore(s, n); }; s.type = "text/javascript"; s.async = true; s.src = "https://mc.yandex.ru/metrika/watch.js"; if (w.opera == "[object Opera]") { d.addEventListener("DOMContentLoaded", f, false); } else { f(); } })(document, window, "yandex_metrika_callbacks"); </script> <noscript><div><img src="https://mc.yandex.ru/watch/41352714" style="position:absolute; left:-9999px;" alt="" /></div></noscript> <!-- /Yandex.Metrika counter -->
</div>
