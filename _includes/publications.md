<h2 id="publications" style="margin: 2px 0px -15px;">Publications</h2>

<div class="publications">
<ol class="bibliography">

{% for link in site.data.publications.main %}

<li>
<div class="pub-row">
  <div class="col-sm-3 col-xs-12">
    {% if pub.video %}
      <video
        autoplay
        loop
        muted
        playsinline
        class="img-responsive img-rounded"
        style="width: 100%; height: auto;"
        poster="{{ pub.image | relative_url }}" >
        <sourcesrc="{{ pub.video | relative_url }}" type="video/mp4" />
        Your browser does not support the video tag.
      </video>
    {% elsif pub.image %}
      {% if pub.code %}
        <a href="{{ pub.code }}" target="_blank">
          <imgsrc="{{ pub.image | relative_url }}" class="img-responsive img-rounded" alt="image" />
        </a>
      {% else %}
        <imgsrc="{{ pub.image | relative_url }}" class="img-responsive img-rounded" alt="image" />
      {% endif %}
    {% endif %}
  </div>
        
  <div class="col-sm-9" style="position: relative;padding-right: 15px;padding-left: 20px;">
      <div class="title"><a href="{{ link.pdf }}">{{ link.title }}</a></div>
      <div class="author">{{ link.authors }}</div>
      <div class="periodical"><em>{{ link.conference }}</em>
      </div>
    <div class="links">
      {% if link.pdf %} 
      <a href="{{ link.pdf }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">PDF</a>
      {% endif %}
      {% if link.code %} 
      <a href="{{ link.code }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Code</a>
      {% endif %}
      {% if link.dataset %} 
      <a href="{{ link.dataset }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Dataset</a>
      {% endif %}      
      {% if link.page %} 
      <a href="{{ link.page }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Project Page</a>
      {% endif %}
      {% if link.bibtex %} 
      <a href="{{ link.bibtex }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">BibTex</a>
      {% endif %}
      {% if link.notes %} 
      <strong> <i style="color:#e74d3c">{{ link.notes }}</i></strong>
      {% endif %}
      {% if link.others %} 
      {{ link.others }}
      {% endif %}
    </div>
  </div>
</div>
</li>
<br>

{% endfor %}

</ol>
</div>
