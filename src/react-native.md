# React Native

## flexbox

- `flex` can use used on BOTH parent AND child.
  - When used on parent as `flex: 1`, it means take all available space.
  - When used on child, it means proportionally distribute all available space assigned to it, together with other child elements under the same parent.
- `flexDirection` only refers to the child elements of whatever the style is on.

```js
<View style={{ flex: 1 }}>
  <View
    style={{ flex: 2, flexDirection: 'row', backgroundColor: 'powderblue' }}
  >
    <View style={{ flex: 2, backgroundColor: 'red', margin: 1 }} />
    <View style={{ flex: 5, backgroundColor: 'red', margin: 1 }} />
  </View>
  <View style={{ flex: 3, backgroundColor: 'skyblue' }} />
  <View style={{ flex: 2, backgroundColor: 'steelblue' }} />
</View>
```

## SafeAreaView

`<SafeAreaView style={styles.container}>` only works for ios.

For android, need this:

`import {Platform, SafeAreaView, StatusBar} from 'react-native'`

`paddingTop: Platform.OS === 'android' ? StatusBar.currentHeight : 0,`
