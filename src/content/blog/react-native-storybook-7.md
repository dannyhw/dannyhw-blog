---
title: "React Native Storybook 7"
description: "Aligning React Native and core Storybook more closely than ever before"
pubDate: "Feb 02 2024"
heroImage: "/rn-storybook-hero.jpg"
---

Storybook is an industry-leading, open source workshop for developing, documenting, and testing frontend applications in isolation. It helps you develop hard-to-reach states and edge cases without needing to run the whole app.

Today, we’re thrilled to announce our latest major release for React Native users: **React Native Storybook 7**! This aligns RN Storybook to core Storybook 7.6, enhances code re-use between `@storybook/react` and `@storybook/react-native`, and builds the foundations for more frequent and streamlined future releases.

📖 Storybook 7 compatibility
🧩 Improved support for Component Story Format 3
🔒 TypeScript-first
💨 Automatic story loading
🛡️ Improved error handling
📝 Enhanced markdown support
🎛️ Auto-generation for controls

## Storybook 7 compatibility

Storybook 7 overhauled its core data structures to optimize bundling, which enabled code splitting and lazy compilation. It also made the code easier to reuse outside of a web context.

While RN Storybook 6.5 shared much of the same code with the core, RN Storybook 7 is based on the core entirely. This means that RN storybook will inherit core improvements as they become available, and makes it easier for RN Storybook to keep up with core in the future. Spoiler alert: Storybook 8 is coming very soon. Watch this space!

## Improved support for Component Story Format 3

RN Storybook 7 stories now look exactly the same as Storybook core. Story types now derive directly from `@storybook/react`.

```tsx
import type { Meta, StoryObj } from "@storybook/react"
import Button from "./Button"

const meta = {
  component: Button,
}

satisfies Meta<typeof Button>

export default meta;

type Story = StoryObj<typeof meta>;

export Basic: Story = {}
```

At the same time, you can use the same `StoryObj` and `Meta types` as core Storybook. This follows improved support for [CSF3](https://storybook.js.org/blog/storybook-csf3-is-here/) and provides stronger type safety and autocompletion. [Learn more](https://storybook.js.org/blog/improved-type-safety-in-storybook-7/).

## TypeScript-first configuration
TypeScript has become the language of choice across Storybook users and the JS ecosystem. RN Storybook 7 follows suit by updating all config files and templates to be TypeScript-first.

RN Storybook 7’s main.ts:

```ts
// .storybook/main.ts
import { StorybookConfig } from '@storybook/react-native';

const main: StorybookConfig = {
  stories: [
    '../components/**/*.stories.?(ts|tsx|js|jsx)',
  ],

  addons: [
    '@storybook/addon-ondevice-notes',
    '@storybook/addon-ondevice-controls',
    '@storybook/addon-ondevice-backgrounds',
    '@storybook/addon-ondevice-actions',
  ],
};

export default main;
```

## Automatic story loading

RN Storybook 7 capitalizes on [Metro](https://docs.expo.dev/guides/customizing-metro/)’s new support for `require.context` to automate story loading and dramatically reduce the steps needed to create stories.

In RN Storybook 6, you had to run `storybook-generate` each time you added a story. Now, with `require.context`, you only need to run generation when you update the `stories` field of `main.ts`. This is because `require.context` imports files dynamically based on a file glob (regex for file paths).

This feature was added in React Native 0.72. Use it by enabling `transformer.unstable_useRequireContext` in Metro:

```js
// metro.config.js

const config = {
  transformer: {
    unstable_allowRequireContext: true,
  },
}
```

[Read more about this on my Dev.To](https://dev.to/dannyhw/dynamic-imports-supported-in-react-native-273j).

## Significantly improved error handling

RN Storybook 7 fixes the issue of Storybook crashing upon story errors. Now, we wrap each story in an error boundary, which catches those errors instead. Note that you may still have to dismiss the RN Error LogBox.

## Enhanced markdown support

In the past, [the Notes addon](https://www.npmjs.com/package/@storybook/addon-ondevice-notes) broke text in the wrong places, and didn’t behave as you’d expect. RN Storybook 7 replaces the markdown renderer with a more battle-tested alternative, `react-native-markdown-display`, which works significantly better!

![storybook notes addon](https://storybookblog.ghost.io/content/images/2024/02/rn-markdown-1.png)

## Auto-generation for controls

RN Storybook 7 now automatically generates controls from types using docgen analysis!

Configure auto-generated controls by adding `babel-plugin-react-docgen-typescript` to your Babel config:

![storybook controls](https://storybookblog.ghost.io/content/images/2024/02/rn-controls.png)

## Get started

Get started with RN Storybook 7 in an existing project with:

```bash
npx storybook@latest init
```

If you’re creating a fresh project, use the Storybook template:

```bash
# Expo
npx create-expo-app --template expo-template-storybook AwesomeStorybook

# React Native CLI
npx react-native init MyApp --template react-native-template-storybook
```

### How to upgrade

There are some breaking changes in this release. Since there’s a lot to go through, [please check out the full migration guide](https://github.com/storybookjs/react-native/blob/next/MIGRATION.md#from-version-65x-to-76x).

## What’s next?

Work on RN Storybook 8 is already underway. The basics are functional, but we have plans for many more improvements in the months ahead. Here’s what you can look forward to:

- Redesigned UI
- Improved support for testing
- More control types to better match the web experience
- …and much more!

## Bringing it all together

This release aligns RN Storybook with core Storybook and paves the way for many further improvements in our next major release.

There’s still a massive amount of potential with RN Storybook project and we’d love new contributors to help us get there. Find the project [here](https://github.com/storybookjs/react-native) and share your feedback, interest, or support.

You can find me on [Twitter](https://twitter.com/Danny_H_W) or [GitHub](https://github.com/dannyhw). If you’d like to support my work, consider sponsoring me via [GitHub sponsors](https://github.com/sponsors/dannyhw/).

<div class="w-full flex items-center justify-center">
<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Big news: React Native Storybook 7 is here!<br><br>🤝 Storybook 7 compatibility<br>🧩 Improved CSF3 support<br>🔒 TypeScript-first<br>💨 Automatic story loading<br>🛡️ Stronger error handling<br>📝 Enhanced markdown<br>🎛️ Controls auto-generation<a href="https://t.co/3bwjr2wahr">https://t.co/3bwjr2wahr</a><br><br>🧵🧵🧵</p>&mdash; Storybook (@storybookjs) <a href="https://twitter.com/storybookjs/status/1753388817925705862?ref_src=twsrc%5Etfw">February 2, 2024</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
</div>
