---
title: "Objective 3 - Point-of-Sale Password Recovery"
date: 2021-06-20T00:00:00Z
draft: false
hide_on_homepage: false
---

Going in to the courtyard, we find `Sugarplum Mary`. He tells us that he has a new POS terminal. The only problem is, he can't find the supervisor password. All he can gives us is the POS terminal application.

Once installed, we can open the app, but not much more than that. The first thing is does, it asks for the password. The first thing that jumps at you is the app's icon.

![Icon](/kringle20/santa-electron.png#center)

This hints at me that the applications is probably an [Electron](https://www.electronjs.org/) app. Taking a closer look at the install folder confirms this.

Electron apps are packaged with [asar](https://github.com/electron/asar). Good thing for us is, we can use the same tools to unpack the app.
After installing the required tools, we try unpack the POS' terminal `.asar` file located in `%localappdata%\Programs\santa-shop`.

```bash
asar extract app.asar sourcecode
```

After some looking through source, we can find the following line of code:

```js
const SANTA_PASSWORD = 'santapass';
```

------------------

You can find the solutions to the other challenges [here](/post/kringle20/intro/).