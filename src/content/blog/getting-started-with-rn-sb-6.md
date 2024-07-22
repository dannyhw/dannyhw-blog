---
title: "Getting started with React Native Storybook"
description: "A quick guide to add react native storybook to an existing project"
pubDate: "August 11 2023"
heroImage: "/gettingstarted.png"
---

This guide is a written version of a video I recently made and will show you how to get started with react native storybook on an existing project. This assumes that you already have a react native project to start with. For my purposes I'm starting with the expo typescript template but you can use react native cli if you prefer.

Heres the video if you want to watch me do it and talk through the process

<iframe width="100%" height="480" style="margin: 1rem 0;" src="https://www.youtube.com/embed/A_bGhCoGXmk?si=npe_AUJSgvmG5gZm" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

The first thing you'll want to do is open the terminal in your project folder and run

```sh
npx sb@latest init --type react_native
```

This will create all the files and install all the packages you need to get started.

![terminal showing command](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6s6cjc2t00lhqlxnq7rh.png)

As you can see from the message here the setup isn't fully automated and the next step is to get storybook to render by replacing your entry point with the Storybook component.

Lets now take a look at what got added to our project

![vscode showing changes](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/m24o98dswgxiqm29rt1p.png)

There were some new packages installed, two new scripts and a `.storybook` folder which contains all the storybook config. The `.storybook/index.js` file exports the Storybook component which we need to render to get storybook working.

Now in your editor open up `App.tsx` (or js/jsx) or wherever your app entrypoint and import and export the component `.storybook/index.js` like you would normally export the App component. For now you can comment out your existing default export but later you can setup a way to swap between storybook and your app.

```js
import StorybookUI from "./.storybook";

export default StorybookUI;
```

![vscode showing code snippet](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y9m5pzhyuvdygy0tps1e.png)

Next we'll make one quick change to our metro config.

If you're on expo you'll need to generate that file with

```sh
npx expo customize metro.config.js
```

Then in that file add sb modern to the start of the resolver.resolverMainFields array. In expos generated file you can do it like this.

```js
// Learn more https://docs.expo.io/guides/customizing-metro
const { getDefaultConfig } = require("expo/metro-config");

/** @type {import('expo/metro-config').MetroConfig} */
const config = getDefaultConfig(__dirname);

config.resolver.resolverMainFields.unshift("sbmodern");

module.exports = config;
```

_note: you'll be happy to know sbmodern won't be needed in v7 of storybook_

Now you can run `yarn ios` or `yarn android` and when the app opens up you should see the storybook UI.

![simulator with storybook ui](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zesi8alcdvlhmqrkzxk5.png)

## Why aren't my new stories showing up?

When you add new stories you might notice that they don't immediately show up, thats because currently you need to run `yarn storybook-generate` each time you add a new story.

I like to add it to the start command like this so that I never forget

`"start": "yarn storybook-generate && expo start",`

This is needed because until recently react native didn't support dynamic imports and we generate a list of story imports based on your `main.js` config. You can see this in the `storybook.requires.js` file. This is something that we'll be changing in the future but for now make sure that you run `storybook-generate` whenever your new stories aren't showing up.

## Thats it folks

Thanks for reading this post, if you're wondering where to go from here I recommend taking a look at some of my other posts or following the [tutorial](https://storybook.js.org/tutorials/intro-to-storybook/react-native/en/get-started/) I wrote for react native storybook.

You can find me on twitter here [@Danny_H_W](https://twitter.com/Danny_H_W)
You can find my work on github here [dannyhw](https://github.com/dannyhw)

My DM's are open on twitter in case you want to get in touch.
