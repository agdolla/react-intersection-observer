<p align="center">
  <img alt="React Intersection Observer" src=".github/logo.svg" width="888">
</p>

<p align="center">
  <a href="https://travis-ci.org/researchgate/react-intersection-observer"><img alt="Build Status" src="https://travis-ci.org/researchgate/react-intersection-observer.svg?branch=master"></a>
  <a href="https://codecov.io/gh/researchgate/react-intersection-observer"><img alt="Codecov" src="https://img.shields.io/codecov/c/github/researchgate/react-intersection-observer.svg"></a>
  <a href="https://greenkeeper.io/"><img alt="Greenkeeper badge" src="https://badges.greenkeeper.io/researchgate/react-intersection-observer.svg"></a>
  <a href="https://www.npmjs.com/package/@researchgate/react-intersection-observer"><img alt="NPM version" src="https://img.shields.io/npm/v/@researchgate/react-intersection-observer.svg"></a>
  <a href="https://github.com/prettier/prettier"><img alt="styled with prettier" src="https://img.shields.io/badge/styled_with-prettier-ff69b4.svg"></a>
</p>

<br>

> Bring ReactIntersectionObserver over today, your React children will love it!

**React Intersection Observer** is a **React** component, acting as a wrapper for the **IntersectionObserver API**. It is fully declarative and takes care of all the imperative parts for you.

**React Intersection Observer** is good at:

* **reusing instances**: comparing the passed options
* **performance**: chooses smartly when to re-render and when to re-observe
* **being unopinionated**: how to handle visibility changes is left entirely up to the developer
* **being intuitive**: looks like the Native API

<br>

<details>
<summary><strong>Table of Contents</strong></summary>

* [What does IntersectionObserver do?](#what-does-intersectionobserver-do)
* [Getting started](#getting-started)
  + [Installation](#installation)
  + [Inside your codebase](#inside-your-codebase)
* [Why ReactIntersectionObserver?](#why-reactintersectionobserver)
  + [No bookkeeping](#no-bookkeeping)
  + [No extra markup](#no-extra-markup)
  + [Easy to adopt](#easy-to-adopt)
* [Documentation](#documentation)
* [Options](#options)
* [Notes](#notes)
* [Polyfill](#polyfill)
* [IntersectionObserver's Browser Support](#intersectionobservers-browser-support)
* [Contributing](#contributing)
* [License](#license)
</details>

---

## What does IntersectionObserver do?

> IntersectionObservers calculate how much of a target element overlaps (or "intersects with") the visible portion of a page, also known as the browser's "viewport":
>
> \- [Dan Callahan](https://hacks.mozilla.org/2017/08/intersection-observer-comes-to-firefox/)&nbsp;&middot;&nbsp;<a href="https://creativecommons.org/licenses/by-sa/3.0/">
<img id="licensebutton_slim" alt="Creative Commons License" src="https://i.creativecommons.org/l/by-sa/3.0/80x15.png" style="margin-right:10px;margin-bottom:4px; border: 0;"></a>

![Graphic example](https://hacks.mozilla.org/files/2017/08/Blank-Diagram-Page-1.png)

## Getting started

```shell
npm install --save @researchgate/react-intersection-observer
```

Optionally add the polyfill and make sure it's required on your dependendencies for unsupporting browsers:

```shell
npm install --save intersection-observer
```

Usage:

```jsx
import React from 'react';
import 'intersection-observer'; // optional polyfill
import Observer from '@researchgate/react-intersection-observer';

class ExampleComponent extends React.Component {
    handleIntersection(event) {
        console.log(event.isIntersecting);
    }

    render() {
        const options = {
            onChange: this.handleIntersection
            root: "#scrolling-container"
            rootMargin: "0% 0% -25%"
        };

        return (
            <div id="scrolling-container" style={{ overflow: 'scroll', height: 100 }}>
                <Observer {...options}>
                    <div>
                        I am the target element
                    </div>
                </Observer>
            </div>
        );
    }
}
```

## Why ReactIntersectionObserver?

The motivation is to provide the easiest possible solution for observing elements that enter the viewport on your **React** codebase. It's fully declarative and all complexity is abstracted away, focusing on reusability, and low memory consumption.

### No bookkeeping

It's built with compatibility in mind, adhering 100% to the [native API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#Intersection_observer_options) implementation and DSL, but takes care of all the bookkeeping work for you.

Instances and nodes are managed internally so that any changes to the passed options or tree root reconciliation cleans up and re-observes nodes on-demand to avoid any unexpected memory leaks.

### No extra markup

ReactIntersectionObserver does not create any extra DOM elements, it attaches to the only child you'll provide to it. Internally it warns you if attaching a `ref` to it fails - common mistake when using a stateless component in React 15 - and will invoke any existing `ref` callback upon the passed child element.

### Easy to adopt

When using ReactIntersectionObserver the only required prop is the `onChange` function. Any changes to the visibility of the element will invoke this callback, just like in the [native API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#Targeting_an_element_to_be_observed) - you’ll receive one `IntersectionObserverEntry` argument per change. This gives you an ideal and flexible base to build upon.

Some of the things you may want to use ReactIntersectionObserver for:

* Determining advertisement impressions
* Lazy loading - Images, or anything that will enter the viewport
* Occlusion culling - Don't render an object until is close to the viewport edges
* [Sentinel Scrolling - Infinite scroller with a recycled Sentinel](https://github.com/researchgate/react-intersection-list)

## Documentation

### Demos

Find multiple examples and usage guidelines under: [https://researchgate.github.io/react-intersection-observer/](https://researchgate.github.io/react-intersection-observer/)

[![demo](https://github.com/researchgate/react-intersection-observer/blob/master/.github/demo.gif?raw=true)](https://researchgate.github.io/react-intersection-observer/)

### Recipes

Recipes are useful code snippets solutions to common problems, for example, how to use ReactIntersectionObserver within a [Higher Order Component](https://researchgate.github.io/react-intersection-observer/?selectedKind=Recipes&selectedStory=Higher%20Order%20Component).

Discover more recipes in our [examples section](examples/README.md).

### Options

**root**: `HTMLElement|string` | default `window object`

The element or selector string that is used as the viewport for checking visibility of the target.

**rootMargin**: `string` | default `0px 0px 0px 0px`

Margin around the root. Specify using units _px_ or _%_ (top, right, bottom left). Can contain negative values.

**threshold**: `number|Array<number>` | default: `0`

Indicates at what percentage of the target's visibility the observer's callback should be executed. If you only want to detect when visibility passes the 50% mark, you can use a value of 0.5. If you want the callback run every time visibility passes another 25%, you would specify the array [0, 0.25, 0.5, 0.75, 1].

**onlyOnce**: `boolean` | default: `false`

When true indicate that events fire only until the element is intersecting. Requires `IntersectionObserverEntry`'s object to contain `isIntersecting` in its prototype.

**disable**: `boolean` | default: `false`

Controls whether the element should stop being observed by its IntersectionObserver instance. Useful for temporarily disabling the observing mechanism and restoring it later.

**onChange** (required): `(entry: IntersectionObserverEntry) => void`

Function that will be invoked whenever an observer's callback contains this target in its changes.

**children** (required): `React.Element<*>`

Single React component or element that is used as the target (observable).

### Notes

* According to the spec, a first callback invocation occurs when the target is first attached to the observer.
* Changes happen asynchronously, similar to the way `requestIdleCallback` works.
* Although you can consider callbacks immediate - always below 1 second - you can also get an immediate response on an element's visibility with `observer.takeRecords()`.

## Polyfill

When needing the full spec's support, we highly recommend using the [IntersectionObserver polyfill](https://github.com/WICG/IntersectionObserver/tree/gh-pages/polyfill).

### Caveats

#### Ealier Spec

Earlier preview versions of [Edge](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/12156111/) and prior to version 58 of [Chrome](https://bugs.chromium.org/p/chromium/issues/detail?id=713819#c8), the support for `isIntersecting` was lacking. This property was added to the spec later and both teams where unable to implement it earlier.

If you are using the prop `onlyOnce` or you need this field within your `IntersectionObserverEntry` instances, make sure to [require the version ^0.4.0](https://github.com/WICG/IntersectionObserver/commit/d4a699cc7a2d12e4e67cd6d1e748bed8bb6e3d8c).

#### Performance issues

As the above-mentioned polyfill doesn't perform callback invocation [asynchronously](https://github.com/WICG/IntersectionObserver/issues/225), you might want to decorate your `onChange` callback with a `requestIdleCallback` or `setTimeout` call to avoid a potential performance degradation:

```js
onChange = (entry) => requestIdleCallback(() => this.handleChange(entry));
```

## [**IntersectionObserver**'s Browser Support](https://platform-status.mozilla.org/)

### Out of the box

<table>
    <tr>
        <td>Chrome</td>
        <td>51 <sup>[1]</sup></td>
    </tr>
    <tr>
        <td>Firefox (Gecko)</td>
        <td>55 <sup>[2]</sup></td>
    </tr>
    <tr>
        <td>MS Edge</td>
        <td>15</td>
    </tr>
    <tr>
        <td>Internet Explorer</td>
        <td>Not supported</td>
    </tr>
    <tr>
        <td>Opera <sup>[1]</sup></td>
        <td>38</td>
    </tr>
    <tr>
        <td>Safari</td>
        <td>Safari Technology Preview</td>
    </tr>
    <tr>
        <td>Chrome for Android</td>
        <td>59</td>
    </tr>
    <tr>
        <td>Android Browser</td>
        <td>56</td>
    </tr>
    <tr>
        <td>Opera Mobile</td>
        <td>37</td>
    </tr>
</table>

* [1] [Reportedly available](https://www.chromestatus.com/features/5695342691483648), it didn't trigger the events on initial load and lacks `isIntersecting` until later versions.
* [2] This feature was implemented in Gecko 53.0 (Firefox 53.0 / Thunderbird 53.0 / SeaMonkey 2.50) behind the preference `dom.IntersectionObserver.enabled`.

### Using polyfill

<table>
  <tr>
    <td>Safari</td>
    <td>6+</td>
  </tr>
  <tr>
    <td>Internet Explorer</td>
    <td>7+</td>
  </tr>
  <tr>
    <td>Android</td>
    <td>4.4+</td>
  </tr>
</table>

## Contributing

We'd love your help on creating React Intersection Observer!

Before you do, please read our [Code of Conduct](.github/CODE_OF_CONDUCT.md) so you know what we expect when you contribute to our projects.

Our [Contributing Guide](.github/CONTRIBUTING.md) tells you about our development process and what we're looking for, gives you instructions on how to issue bugs and suggest features, and explains how you can build and test your changes.

**Haven't contributed to an open source project before?** No problem! [Contributing Guide](.github/CONTRIBUTING.md) has you covered as well.
