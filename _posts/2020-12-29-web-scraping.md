---
layout: post
title: Web Scraping
date: 2020-12-29 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: web-scraping.jpeg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Beutifulsoup]
---
Kada radimo na nekom Data Science projektu, uobičajeno je da želimo da koristimo podatke koji se nalaze na internetu. Obično te podatke nalazimo u csv formatu ili preko Application Programming Interface (API). Međutim, postoje slučajevi kada se podacima kojima nam trebaju može pristupiti samo kao deo Web stranice. U ovakvim slučajevima možemo da koristimo tehniku koja se naziva Web scraping, da bismo dobili podatke sa Web stranice u format sa kojim možemo raditi u našoj analizi.
Web scraping je tehnika kojom se automatski pristupa i izdvaja velika količina informacija sa web stranice, što može uštedeti ogromnu količinu vremena i truda.

Napravićemo jedan jednostavan primer kako možemo automatizovati preuzimanje stotinu fajlova iz New York MTA.
## Python code
Osnovne biblioteke za Web Scraping koje moramo uključiti su sledeće:

{% highlight ruby %}
import requests
import urllib.request
import time
from bs4 import BeautifulSoup
{% endhighlight %}

Sledeće što moramo da uradimo je da navedemo url do strane koju skrejpujemo.
{% highlight ruby %}
url = 'http://web.mta.info/developers/turnstile.html'
response = requests.get(url)
{% endhighlight %}

Sada je došao trenutak da našu stranu parsiramo sa BeautifulSoup
{% highlight ruby %}
soup = BeautifulSoup(response.text, “html.parser”)
{% endhighlight %}
Pošto smo već videli u source-u strane da se podaci koji nama trebaju nalaze u ‘a’ tagu, upotrebićemo .findAll metodu da pokupimo tekst iz ovih tagova.
{% highlight ruby %}
soup.findAll('a')
{% endhighlight %}


>Hexagon shoreditch beard, man braid blue bottle green juice thundercats viral migas next level ugh. Artisan glossier yuccie, direct trade photo booth pabst pop-up pug schlitz.


* Hexagon shoreditch beard
* Intelligentsia narwhal austin
* Literally meditation four
* Microdosing hoodie woke

