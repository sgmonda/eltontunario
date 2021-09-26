---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
image: /img/album-art.jpg
---

<div class="has-text-centered">
  <img src="/img/logo-big.png" width="256" height="256" style="margin-top: 3em"/>
  <hr/>
  {% for ep in site.posts %}
    <div>
      <p>
        <small style="color: #aaa">{{ ep.date | date: "%d %b %Y" }}</small><br/>
        <a href="{{ ep.mp3 }}" target="_blank">{{ ep.title }}</a>
      </p>
      <!-- <audio controls>
        <source src="{{ ep.mp3 }}" type="audio/ogg">
      </audio> -->
      <!-- <p class="is-size-7">
        <a href="{{ ep.mp3 }}" target="_blank">Descargar</a>
      </p> -->
      <hr/>
    </div>
  {% endfor %}
</div>
