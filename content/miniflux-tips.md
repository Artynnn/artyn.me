+++
title = "Miniflux for Google Reader fans"
date = 2025-10-10
draft = false
topic = "Web"
+++

RSS is the best way to follow anything on the internet. It centralizes all of the disparate websites into one place. However one of the easier ones  to self-host, Miniflux, has a really annoying "river of news" style feed. Which becomes impossible to follow if you are subscribed to a ton of feeds.

Miniflux is inspired by the lobsters website but that site gets around 30-70 entries a day. That scale is super manageable. But I regularly have 1000 to 8000 items in my feed, I can't actually follow what I follow.

First I change default home page to categories and then with a bit of Javascript I solves all of the issues.

```javascript
if (document.getElementsByTagName("title")[0].innerHTML == 'Categories (10) - Miniflux')
{
    let categoryTitles = document.getElementsByClassName("item-title");
    for (let i = 0; i < categoryTitles.length; i++) {
        if (categoryTitles[i].querySelector("a").text.includes("webcomics") ||
            categoryTitles[i].querySelector("a").text.includes("videos")    ||
            categoryTitles[i].querySelector("a").text.includes("podcasts"))
        {
            // do nothing
        }
        else
        {
            categoryTitles[i].querySelector("a").href =
            categoryTitles[i].querySelector("a")
            .href.replace(/entries$/, "feeds");
        }
    }
}
```

I also like to keep my feeds as compact as possible which makes scanning easier

```css
.feed-parsing-error {
    display: none;
}

.item-meta {
    display: none;
}

.item {
  margin-bottom: 0px;
}

.category > a {
  display: none;
}

.category {
  border: none;
  background-color: none;
}
```

