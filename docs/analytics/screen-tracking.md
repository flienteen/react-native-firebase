---
title: Screen Tracking
description: Setup Firebase Analytics to track your in-app screen flow.
previous: /analytics/usage
next: /
---

Standard React Native applications run inside a single `Activity`/`ViewController`, meaning any screen changes won't be
tracked by the native Firebase SDKs. There are a number of ways to implement navigation within React Native apps,
therefore there is no "one fits all" solution to screen tracking.

# React Navigation

The [React Navigation](https://reactnavigation.org/) library allows for various navigation techniques such as
Stack, Tab, Native or even custom navigation. The `NavigationController` component which the library exposes provides
access to the current navigation state when a screen changes, allowing you to use the [`setCurrentScreen`](/reference/analytics#setcurrentscreen)
method the Analytics library provides:

```jsx
import analytics from '@react-native-firebase/analytics';
import { NavigationContainer } from '@react-navigation/native';

<NavigationContainer
  ref={navigationRef}
  onStateChange={state => {
    const previousRouteName = routeNameRef.current;
    const currentRouteName = getActiveRouteName(state);

    if (previousRouteName !== currentRouteName) {
      analytics().setCurrentScreen(currentRouteName, currentRouteName);
    }
```

For a full working example, view the [Screen tracking for analytics](https://reactnavigation.org/docs/screen-tracking/)
documentation on the React Navigation website.

# React Native Navigation

The [`wix/react-native-navigation`](https://github.com/wix/react-native-navigation) provides 100% native platform navigation
for React Native apps. To manually track screens, you need to setup a command listener and manually call the
[`setCurrentScreen`](/reference/analytics#setcurrentscreen) method the Analytics library provides:

```js
import analytics from '@react-native-firebase/analytics';
import { Navigation } from 'react-native-navigation';

Navigation.events().registerCommandListener((name, params) => {
  if (name === 'push') {
    analytics().setCurrentScreen(params.componentId, params.componentId);
  }
});
```

To learn more, view the [events documentation](https://wix.github.io/react-native-navigation/#/docs/events?id=registercommandlistener)
on the React Native Navigation website.
