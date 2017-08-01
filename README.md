# [React Intersection Observer](https://Rendez.github.io/react-intersection-observer/) &middot; [![Build Status](https://travis-ci.org/researchgate/react-intersection-observable.svg?branch=master)](https://travis-ci.org/researchgate/react-intersection-observable) [![NPM version](https://img.shields.io/npm/v/@researchgate/react-intersection-observer.svg)](https://www.npmjs.com/package/@researchgate/react-intersection-observer) [![Downloads](https://img.shields.io/npm/dw/@researchgate/react-intersection-observer.svg)]() [![styled with prettier](https://img.shields.io/badge/styled_with-prettier-ff69b4.svg)](https://github.com/prettier/prettier) [![License](https://img.shields.io/npm/l/express.svg)](LICENSE)

> Bring ReactIntersectionObserver over today, your React children will love it!

<br>

**React** component for the **IntersectionObserver API**.

ReactIntersectionObserver is good at:

* **reusing instances**: comparing the passed options
* **performance**: chooses smarly when to re-render and when to re-observe
* **being unopinionated**: how to handle visibility changes is left entirely up to the developer
* **being intuitive**: looks like the Native API

## Getting started

#### Installation

Add this repository to your dependencies:

```bash
npm install --save @researchgate/react-intersection-observer
```

Optionally add the polyfill and make sure it's required on your dependendencies for unsupporting browsers:

```bash
npm install --save intersection-observer
```

#### Inside your codebase
```jsx
import 'intersection-observer'; // adding optional polyfill
import Observer from '@rg/react-Intersection-observer';

class Component extends React.Component {
    handleIntersection = (event) => {
        if (event.isIntersecting) {
            console.log('I am intersecting ');
        } else {
            console.log('I am NOT intersecting');
        }
    };

    render() {
        const options = {
            onChange: this.handleIntersection
            root: "#scrolling-container"
            rootMargin: "0% 0% -25% 0%"
        };

        return (
            <div id="scrolling-container" style={{ overflow: 'scroll', height: 100 }}>
                <Observer {...options}>
                    <div>
                        I should intersect with the viewport
                    </div>
                </Observer>
            </div>
        );
    }
}
```

## Why ReactIntersectionObserver?

The purpose of ReactIntersectionObserver is to provide the easiest possible solution for observing elements that enter the viewport on your **React** codebase. It's fully declarative and all complexity is abstracted away, focusing on reusability, and low memory consumption.

### No bookkeeping

It's build with compatibility in mind, adhering 100% to the [native API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#Intersection_observer_options) implementation and DSL, but without any of the additional bookkeeping work.

Instances and nodes are managed internally so that any changes to the passed options or tree root reconcilliation cleans up and re-observes nodes on-demand to avoid any unexpected memory leaks.

### No extra markup

ReactIntersectionObserver does not create any extra elements, it attaches to the only child you'll provide to it. Internally it warns you if attaching a `ref` to it fails - common mistake when using a stateless component - and will invoke any exisiting `ref` callback upon the passed child element.

### Easy to adopt

When using ReactIntersectionObserver the only required prop is the `onChange` function. Any changes to the visibility of the element will invoke this callback, just like in the [native API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#Targeting_an_element_to_be_observed) - you’ll receive one `IntersectionObserverEntry` argument per change. This gives you an ideal and flexible base to build upon.

Some of the things you may want to use ReactIntersectionObserver for:

* Determining advertisement impressions
* Lazy loading - Images, or anything that will enter the viewport
* Occlusion culling - Don't render an object until is close to the viewport edges
* Sentinel Scrolling - Infinite scroller with a recycled Sentinel

## Options

### root

Type: `Element|string`
Default: `window object`

The element or selector string that is used as the viewport for checking visibility of the target.

### rootMargin

Type: `string`
Default: `0px 0px 0px 0px`

Margin around the root. Specify using units _px_ or _%_ (top, right, bottom left). Can contain negative values.

### threshold

Type: `number|Array<number>`
Default: `0`

Indicates at what percentage of the target's visibility the observer's callback should be executed. If you only want to detect when visibility passes the 50% mark, you can use a value of 0.5. If you want the callback run every time visibility passes another 25%, you would specify the array [0, 0.25, 0.5, 0.75, 1].

### onlyOnce

Type: `boolean`
Default: `false`

When true indicate that events fire only until the element is intersecting. Requires IntersectionObserverEntry's object to contain `isIntersecting` in its prototype.

### disable

Type: `boolean`
Default: `false`

Controls whether the element should stop being observed by its IntersectionObserver instance. Useful for temporarily disabling the observing mechanism and restoring it later.

### onChange : required

Type: `Function`

Function that will be invoked whenever the intersection value for this element changes.

### children : required

Type: `element|component`

Single React component or element that is used as the target to observe.

## Demo

Find full demos in: [https://Rendez.github.io/react-intersection-observer/](https://Rendez.github.io/react-intersection-observer/).

## Polyfill

When needing the full spec's support, we highly recommend using the [IntersectionObserver polyfill](https://github.com/WICG/IntersectionObserver/tree/gh-pages/polyfill).

### Caveats

#### Ealier Spec

Earlier preview versions of [Edge](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/12156111/) and prior to version 58 of [Chrome](https://bugs.chromium.org/p/chromium/issues/detail?id=713819#c8), the support for `isIntersecting` was lacking. This property was added to the spec later and both teams where unable to implement it earlier. If you are using the prop `onlyOnce` or you need this field within your IntersectionObserverEntry instances, you'll want to do some feature detection or follow some of the recipes provided in [this thread](https://github.com/WICG/IntersectionObserver/issues/211) beforehand.

#### Performance issues

Insofar the abovementioned polyfill doesn't perform callback invokation [asynchoronously](https://github.com/WICG/IntersectionObserver/issues/225), you might want to decorate your `onChange` callback with a `requestIdleCallback` or `setTimeout` call to avoid a potential performance degradation:

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

Please read [CONTRIBUTING.md](.github/CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details