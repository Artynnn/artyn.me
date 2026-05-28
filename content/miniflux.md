+++
title = "Making Miniflux more usable"
author = ["Artyn"]
date = 2026-03-08
lastmod = 2026-05-27T22:55:49-05:00
draft = false
weight = 2001
topic = "Self-hosting"
description = "Making Miniflux the best RSS reader."
+++

Miniflux was originally my least favorite RSS newsreader. But after learning you can inject arbitrary CSS and JS it is now my favorite. I think the default experience of Miniflux (and by extension most RSS readers) sucks and we can do better.

The biggest flaw of RSS readers is that they treat it like e-mail or in the case of Miniflux they treat it like Reddit sorted by "new". How I use RSS is that I use it to subscribe to things that interest me. I don't care to read everything written by XYZ on XYZ date. I might read it 3 months after it was written, it really doesn't bother me. To me RSS should not be something stressful and that you have to look at every item. Some have talked about RSS  promoting "phantom obligations". I treat it as a database of interesting stuff.

I like to put everything into categories. Usually by topic since I only care to read about X topic when I am in the mood for it. I have some JavaScript that opens it in "river of news" or "feed list view" depending on the category.

The default experience of Miniflux is awful. It is based on a website (Lobste.rs) that usually gets like 10 items posted a day. If you just naturally follow stuff that you find interesting it is very easy to be like me and have 80,000 unread things. I use to think that was a bad thing but in actuality it means I found a lot of interesting stuff on the internet.

Certain aspects of the UI actually suck. The most egregious is that "mark all as read" requires two taps and is a very small button. Its not so bad on desktop since I usually just hit the keyboard shortcut but it is absolutely painful on mobile. I improved the situation by adding a giant button that I just have to press once.

I also think there is too much extraneous information and that there is too much much padding. Most of the time date doesn't matter to me. It is not like a forum where replying to a 4 month old story is kinda rude and non-sensical (usually impossible on most websites that lock threads after X days).

I also think you shouldn't look at any number. They are pointless. I don't care if a entry has 353 unread articles or just two.


## Change some defaults in the settings {#change-some-defaults-in-the-settings}

-   remove "all" category. Whats the point?

-   make default home: `categories`

-   entry sorting: `recent entries first`

-   entries per page: a big number like `300`


## Better design {#better-design}

Get rid of numbers and make it more compact.

```css
#page-header-title span {
  display: none;
}

.item-title span {
  display: none;
}

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

#feed-entries-counter {
  display: none;
}

.unread-counter-wrapper {
  display: none;
}
```


## Better behavior {#better-behavior}

Make the default open behavior contextual and more sensible.

```javascript
if (document.getElementsByTagName("title")[0].innerHTML == 'Categories (3) - Miniflux') {
    let categoryTitles = document.getElementsByClassName("item-title");
    for (let i = 0; i < categoryTitles.length; i++) {
        if (categoryTitles[i].querySelector("a").text.includes("comics")    ||
            categoryTitles[i].querySelector("a").text.includes("videos")    ||
            categoryTitles[i].querySelector("a").text.includes("podcasts"))
        {
            // NEVER GONNA GIVE YOU UP
        }
        else
        {
            categoryTitles[i].querySelector("a").href = categoryTitles[i].querySelector("a").href.replace(/entries$/, "feeds");
        }
    }
}
```

Add a button that is useful for marking as read for mobile devices. This code is ugly and I need to make it nicer.

```javascript
// button
function Marky() {
    const markButtons = Array.from(document.querySelectorAll('a, button'))
          .filter(el =>
                  /Mark this page as read/i.test(el.textContent.trim())
                  || el.classList.contains('markPageAsRead')
                  || el.id === 'markPageAsRead'
          );

    markButtons.forEach(btn => {
        btn.click();
    });

    const buttons = document.querySelectorAll('button');

    // Look for one whose text content is "Yes"
    const yesButtons = Array.from(buttons).filter(
        btn => btn.textContent.trim().toLowerCase() === 'yes'
    );

    yesButtons.forEach(btn => {
        btn.click();
    });
}

const buttonz = document.createElement('button');

// Set the button text
buttonz.textContent = 'Mark All as read';

// (Optional) Add a CSS class or styling
buttonz.style.padding = '10px 20px';
buttonz.style.fontSize = '16px';
buttonz.style.borderRadius = '8px';
buttonz.style.cursor = 'pointer';
buttonz.style.background = 'beige';

// (Optional) Add an event listener
buttonz.addEventListener('click', Marky);

// Add the button to the page (for example, inside <body>)
document.body.appendChild(buttonz);
```

I have not even actually touched trying to make any aesthetic UI changes. But Miniflux should make that easy compared to other readers.
