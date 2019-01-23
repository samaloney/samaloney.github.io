Title: Setting Up My Site
Date: 2018-01-21 20:42
Category: Blog
Tags: pelican, publishing
Authors: Shane Maloney
Summary: Creating my personal website.

Preamble
--------

I've been putting off setting up a personal website for far too long. In the past I've looked at various options for creating a personal site such as, [WordPress](https://www.wordpress.org), [Jekyll](https://jekyllrb.com), [Hugo](https://gohugo.io) but none of them really seemed to suite my needs.

WordPress is great and I'm familiar with using it however, it seems a bit of overkill for my purposes. I don't really need dynamic server side functions and I would have to host it somewhere, not a big deal by why bother when it's not necessary.As I don't need any dynamic content I decided to look at static site generators. Also static sites can be hosted on GitHub via [GitHub pages](https://pages.github.com), one less thing to manage.

Jekyll seems to be one of the most popular and well supported, it has lots of nice themes, plugins and is even integrated with GitHub pages. However it is written in [Ruby](https://www.ruby-lang.org/en/), which I'm not too familiar with same goes for its template language [Liquid](https://shopify.github.io/liquid/). Next up Hugo another very popular static site generator with good support and lots of nice themes. Hugo is written in Go, which I'm not that familiar with, or Go's template system.

Enter [Pelican](https://blog.getpelican.com) a python based static site generator which uses [Jina template](http://jinja.pocoo.org) language which I know from working with Django. Pelican doesn't seem to have as large a community as either Jekyll or Hugo buy it also seem to be a little less complex. In the end I've chosen to use Pelican for a number of reasons:

1. Python based with Jina templates
2. Easy integration with GitHub pages
3. Smaller community but seems a bit less complex so will be easier to hack

Lets go!
--------
My first step was to create a isolated environment (pyenv and virtualenv) and get Pelican installed

```bash
mkproject -python3 samloney.com
pip install pelican
pelican-quickstart
```

At this point I could begin to add content look for a theme. As for a theme I wanted something responsive, minimal, looked good on mobile or desktops, and would be easy to modify. I quickly found [pelican-blue](https://github.com/Parbhat/pelican-blue) by [Parbhat Puri](https://github.com/Parbhat) which looked good but I knew I would need to make some modifications so I forked the repo [here](https://github.com/samaloney/pelican-blue)

Next I wanted to add a list of my publications to the site, I could have done this by hand but this seemed like a job to automate. After some initial searching I came across a Pelican plugin called [pelican_publications](https://github.com/adamreeve/pelican_publications) that seemed like it would do the job. It would take a `.bib` file, parse it and return the contents to a template. After a quick install it became apparent that I would need to create the template.

```jina
{% block content %}
    <ul>
    {% for pub in publications %}
        <li>
            {% for author in pub['author']%}
                {{' '.join(author).replace('~', ' ')}}, 
            {% endfor %}
            <em>{{pub['title']}}</em>, 
            {% if pub['journal'] == '\\aap' %}
                A&A, 
            {% elif pub['journal'] == '\\solphys' %}
                Sol. Phys., 
            {% elif pub['journal'] == '\\apjl' %}
                ApJL
            {% else%}
                {{pub['journal']}}
            {% endif %}
            {{pub['year']}}, 
            {{pub['month'].capitalize()}}, 
            {{pub['volume']}}, 
            P {{pub['pages']}},
            {% if pub['eprint'] %}
                <a href="https://arxiv.org/abs/{{pub['eprint']}}">arXiv</a>,
            {% endif %}
            {% if pub['doi'] %}
                <a href="https://doi.org/{{pub['doi']}}">DOI</a>,
            {% endif %}
            {% if pub['adsurl'] %}
                <a href="{{pub['adsurl']}}">ADS</a>
            {% endif %}
        </li>
    {% endfor %}
    </ul>
{% endblock %}

```

It turned out the pelican_publications plugin didn't do what I expected/wanted which made the template more complicated then I wanted, a task for the future, it was good enough for the moment.

Time to Publish
---------------
At this point I figured I might as well try publishing what I had so far so I created a repo with the correct name following the GitHub pages [instructions](https://pages.github.com). I ran `make publish` which runs pelican with `pelicanconf.py` and additionally `publishconf.py` where I set the site name to my github pages url. As suggested in the pelican doc I had installed ghp-import so all I had to do was
```bash
ghp-import <path>/<to>/output
git push orgin gh-pages
```
and it didn't work a quick review of the GitHub Pages docs indicated user pages now had to be in the master branch. Second try
```bash
ghp-import -b master <path>/<to>/output
git push orgin master
```
it worked will kind of, the home page seemed ok, missing some css but every other page was borked. A quick review of browser console indicated issues serving mixed content (http, https) and the links were not generated properly. After a bit of playing around I found the causes:

1. RTFM - from pelican docs `SITEURL` must be full URL with protocol
2. Also with the theme I was using `SITEURL` had at the start of `pelicanconf.py` and didn't work when set in `publishconf.py`

Success!

The code for the site at this stage is [here](https://github.com/samaloney/samaloney.github.io-src)

