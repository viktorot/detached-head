---
title: "Objective 4 - Operate the Santavator"
date: 2021-06-27T00:00:00Z
draft: false
hide_on_homepage: false
---

`Sparkle Redberry` greets us it the hallway of Santa's castle and tells us that there is something wrong with Santa's elevator.
He tells us to look under the S4 (Super Santavator Sparkle Stream) panel and see if we can get the elevator working again.

When we open the panel up we see a bunch of electronic boards and an electronic particle stream, flowing upwards. There are also three differently colored openings.

On top of the electronic board we see items that we picked up as we were walking through the yard and castle. We can use the items to manipulate the particle stream's flow and guide it towards the openings. To get the elevator to work, we need to activate manipulate the particle stream and get it to flow into the colored openings to activate the elevator.

And here is where I managed to solve this objective and an [objective](/post/kringle20/objective10/) that has still not appeared. See, after getting the jest of how the challenge is constructed, first thing I did is make a quick pass of the JS code that runs the elevator logic. First thing I noticed is that the script reads `tokens` from the URL parameters and parses them. These happen to the the items you have found through the castle up to this point.

```js
const getParams = __PARSE_URL_VARS__();
let tokens = (getParams.tokens || '').split(',');
```

Looking further down the source code, we can see a `defaultState` variable. As it turns out, it contains a list of all available tokens/items. So if we update the URL with a list of all the tokens available, we should see them all on the electronic panel. And that is exactly what happens.

```
https://elevator.kringlecastle.com?challenge=santamode-elevator&area=santamode-santavator1&tokens=ball,redlight,elevator-key,nut,nut2,candycane,workshop-button,marble,greenlight,yellowlight
```

Once we get that working, all it's left to do is to re-arrange all the items, so that the particle beam goes to the openings and the elevator activates. As some other write-ups mentioned, you can manipulate the CSS of the item sprites and make the game easily. I decided to play the hard mode and use the items as-is. And here is what I came up with.

![Solution](/kringle20/elevator.png#center)

And with that, the objective is complete

------------------

You can find the solutions to the other challenges [here](/post/kringle20/intro/).