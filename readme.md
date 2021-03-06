# react-native-baidumap-sdk [![npm version][version-badge]][npm] [![build status][build-badge]][build]

React Native BaiduMap SDK for Android + iOS.

你可以下载安装 [example.apk](https://github.com/qiuxiang/react-native-baidumap-sdk/releases/download/v0.3.0/example.apk) 看看实际中的效果。

*注意：RN v0.53 存在一些 bug（主要影响 iOS），建议使用 RN v0.52。*

## 安装

- [安装说明](docs/installation.md)
- [项目示例运行说明](docs/setup.md)

## 用法

### 基本用法
```javascript
import { MapView } from 'react-native-baidumap-sdk'

render() {
  return <MapView center={{ latitude: 39.2, longitude: 112.4 }} />
}
```

### 监听地图事件
```javascript
import { MapView } from 'react-native-baidumap-sdk'

render() {
  return (
    <MapView
      onLoad={() => console.log('onLoad')}
      onClick={point => console.log(point)}
      onStatusChange={status => console.log(status)}
    />
  )
}
```
<img src="https://user-images.githubusercontent.com/1709072/36655486-edc2e0f8-1afd-11e8-942b-22ae7c2db21c.png" width=300>

### 定位并关联定位图层
```javascript
import { MapView, Location } from 'react-native-baidumap-sdk'

await Location.init()
Location.addLocationListener(location => this.setState({ location }))
Location.start()

state = { location: null }

render() {
  return <MapView location={this.state.location} locationEnabled />
}
```
<img src="https://user-images.githubusercontent.com/1709072/36655487-ee218a5e-1afd-11e8-8efd-e2ed99268df5.png" width=300>

### 添加标记
```javascript
<MapView>
  <MapView.Marker
    color="#2ecc71"
    title="This is a marker"
    onPress={this.onPress}
  />
</MapView>
```
<img src="https://user-images.githubusercontent.com/1709072/36655491-f24ab3d0-1afd-11e8-8928-622a624aa850.png" width=300>

### 添加自定义标记
```javascript
<MapView>
  <MapView.Marker
    icon={() => (
      <View>
        <Image source={image} />
        <Text>This is a custom marker</Text>
      </View>
    )}
  />
</MapView>
```
<img src="https://user-images.githubusercontent.com/1709072/36655482-ec5d23b8-1afd-11e8-99c3-bbf62c163476.png" width=300>

### 地理编码/逆地理编码
```javascript
import { Geocode } from 'react-native-baidumap-sdk'

const searchResult = await Geocode.search('海龙大厦')
const reverseResult = await Geocode.reverse({ latitude: 39, longitude: 113 })
```
<img src="https://user-images.githubusercontent.com/1709072/36655485-ed756bfc-1afd-11e8-8f4b-c6ec50ebc8dd.png" width=300>

需要注意，以上例子简写了一些属性，并不能直接使用，更多实际的例子请参考：[example](https://github.com/qiuxiang/react-native-baidumap-sdk/tree/master/example)。

### 点聚合
```javascript
onStatusChange = status => this.cluster.update(status)

renderMarker = item => (
  <MapView.Marker
    key={item.extra.key}
    coordinate={item.coordinate}
  />
)

render() {
  return (
    <MapView onStatusChange={this.onStatusChange}>
      <MapView.Cluster
        ref={ref => this.cluster = ref}
        markers={this.markers}
        renderMarker={this.renderMarker}
      />
    </MapView>
  )
}
```
<img src="https://user-images.githubusercontent.com/1709072/36655484-ed17649e-1afd-11e8-81c5-04a981862b1a.png" width=300> <img src="https://user-images.githubusercontent.com/1709072/36655483-ecbb4b64-1afd-11e8-954c-ded218f8a696.png" width=300>

## 接口文档
[JS 代码](lib/js)有完善的类型标注，建议结合源代码一起阅读，特别是需要知道具体参数、返回值类型的时候。

- [MapView](docs/map-view.md)
  - [Marker](docs/marker.md)
  - [Polygon](docs/polygon.md)
  - [Cluster](docs/cluster.md)
- [Location](docs/location.md)
- [Geocode](docs/geocode.md)

[npm]: https://www.npmjs.com/package/react-native-baidumap-sdk
[version-badge]: https://badge.fury.io/js/react-native-baidumap-sdk.svg
[build-badge]: https://travis-ci.org/qiuxiang/react-native-baidumap-sdk.svg?branch=master
[build]: https://travis-ci.org/qiuxiang/react-native-baidumap-sdk
