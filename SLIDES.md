---
marp: true
footer: Build vs Use
paginate: true
style: |
    table {
        margin-top: 1em;
        align: center;
    }

    h1, h2 {
        text-shadow: 0 0.3em 1em gray;
    }
    
---

# Build Vs Use

**Josh Schneider**
Github: [@dijital20](https://github.com/dijital20) / [talk-build-v-use](https://github.com/dijital20/talk-build-v-use)
Mastodon: [mastodon.social/@diji](https://mastodon.social/@diji)

<!-- 
_footer: ""
_class: invert
 -->

---

In any project of a certain size, you will come across a point where you need a feature that isn't provided by the language or standard library. 

At that point, you will have a choice whether to ***write the functionality yourself*** or ***pull in some hopefully brilliant, well-maintained, secure bit of third party code that does the job for you***.

---

## Anyone remember this movie?

![bg fit right](img/slidingDoorsPoster.png)

---

## What if we built it ourselves?

<!-- 
footer: What if we built it ourselves?
_footer: ""
_class: invert
 -->

---

* Speed of delivery
* Bespoke fit
* Documentation and Support (or lack thereof)
* Bug fixes
* Security updates
* New features

---

## What if we used something?

<!-- 
footer: What if we used something?
_footer: ""
_class: invert
 -->

---

* Speed of delivery (or lack thereof)
* Off-the-peg fit
* Documentation and Support
* Bug fixes
* Security updates
* New features

---

## Reflecting

<!--
footer: Reflecting 
_footer: ""
_class: invert
 -->

---

## Reflection

* Building yourself costs you initial speed for long-term control and customization. *With great power, comes great responsibility*... meaning new features, fixes, and support are all on you.

* Using an existing solution rewards you with immediate velocity, and some built-in support community; but at the cost of control and having to work on someone else's schedule, and giving you the responsibility of keeping up with the solution as it matures to ensure you stay ahead of bugs and security issues.
