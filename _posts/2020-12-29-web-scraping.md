---
layout: post
title: Web Scraping
date: 2019-02-29 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: web-scraping.jpeg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Beutifulsoup]
---
Kada radimo na nekom Data Science projektu, uobičajeno je da želimo da koristimo podatke koji se nalaze na internetu. Obično te podatke nalazimo u csv formatu ili preko Application Programming Interface (API). Međutim, postoje slučajevi kada se podacima kojima nam trebaju može pristupiti samo kao deo Web stranice. U ovakvim slučajevima možemo da koristimo tehniku koja se naziva Web scraping, da bismo dobili podatke sa Web stranice u format sa kojim možemo raditi u našoj analizi.
Web scraping je tehnika kojom se automatski pristupa i izdvaja velika količina informacija sa web stranice, što može uštedeti ogromnu količinu vremena i truda.

Napravićemo jedan jednostavan primer kako možemo automatizovati preuzimanje stotinu fajlova iz New York MTA.

#### Python code

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
soup = BeautifulSoup(response.text, "html.parser")
{% endhighlight %}
Pošto smo već videli u source-u strane da se podaci koji nama trebaju nalaze u <a> tagu, upotrebićemo .findAll metodu da pokupimo tekst iz ovih tagova.
{% highlight ruby %}
soup.findAll('a')
{% endhighlight %}

Na ovaj način smo pokupili sve linije sa strane koje sadrže a tag. Medjutim, pošto se linkovi do fajlova koji nas zanimaju nalaze tek od 36-e linije, onda se možemo fokusirati na samo na linije od nje. Pa ćemo recimo prvom linku koji je nama interesantan pristupiti na sledeći način.

{% highlight ruby %}
one_a_tag = soup.findAll('a')[36]
link = one_a_tag['href']
{% endhighlight %}

I na kraju, da bi uradili download ovih fajlova na naš računar koristićemo biblioteku **urllib.request**.

{% highlight ruby %}
download_url = 'http://web.mta.info/developers/'+ link
urllib.request.urlretrieve(download_url,'./'+link[link.find('/turnstile_')+1:]) 
{% endhighlight %}

Na ovaj način smo uradili download prvog fajla iz liste, "Saturday, February 02, 2019".

Da bi uradili download svih fajlova, možemo prethodni kod ubaciti u jednu for petlju:
{% highlight ruby %}
for i in range(36,len(soup.findAll('a'))+1): 
    one_a_tag = soup.findAll('a')[i] 
    link = one_a_tag['href'] 
    download_url = 'http://web.mta.info/developers/'+ link  
   urllib.request.urlretrieve(download_url,'./'+link[link.find('/turnstile_')+1:])        
    # pauziramo code 1 sekundu zbog spamovanja
    time.sleep(1) 
{% endhighlight %}

I to je sve :blush:
