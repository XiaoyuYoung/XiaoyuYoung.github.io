<li>
<div class="pub-row" style="display: flex; align-items: flex-start; margin-bottom: 20px;">
  <div class="col-sm-3 abbr" style="position: relative; padding-right: 15px; padding-left: 15px; flex-shrink: 0;">
    {% if link.video %}
      <video 
        autoplay loop muted playsinline 
        poster="{{ link.image }}" 
        class="teaser z-depth-1" 
        style="width: 180px; height: 110px; object-fit: cover; border-radius: 4px;"> 
        <source src="{{ link.video }}" type="video/mp4">
      </video>
    {% elsif link.image %}
      <img src="{{ link.image }}" 
        class="teaser z-depth-1" 
        style="width: 180px; height: 110px; object-fit: cover; border-radius: 4px;">
    {% endif %}

    {% if link.conference_short %} 
      <abbr class="badge" style="position: absolute; top: 0; left: 15px;">{{ link.conference_short }}</abbr>
    {% endif %}
  </div>

  <div class="col-sm-9" style="position: relative; padding-right: 15px; padding-left: 20px;">
      <div class="title"><a href="{{ link.pdf }}" target="_blank" style="font-weight: bold;">{{ link.title }}</a></div>
      <div class="author">{{ link.authors }}</div>
      <div class="periodical"><em>{{ link.conference }}</em></div>
    <div class="links" style="margin-top: 8px;">
      {% if link.pdf %} <a href="{{ link.pdf }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px; border: 1px solid #ccc; margin-right: 5px;">PDF</a> {% endif %}
      {% if link.code %} <a href="{{ link.code }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px; border: 1px solid #ccc; margin-right: 5px;">Code</a> {% endif %}
      {% if link.dataset %} <a href="{{ link.dataset }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px; border: 1px solid #ccc; margin-right: 5px;">Dataset</a> {% endif %}
      {% if link.bibtex %} <a href="{{ link.bibtex }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px; border: 1px solid #ccc; margin-right: 5px;">BibTex</a> {% endif %}
      {% if link.notes %} <strong><i style="color:#e74d3c; margin-left: 10px;">{{ link.notes }}</i></strong> {% endif %}
    </div>
  </div>
</div>
</li>
