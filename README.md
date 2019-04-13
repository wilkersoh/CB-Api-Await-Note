
## CallBack
#### 为什么需要使用cb呢？
在fn里他是sync的fn,当你接受api还是执行一些过后才会return值回来的函数就需要使用cb让他把值植入cb里
eg:
``` javascript

const add = (a,b,cb) => {
  setTimeout(() => {
    console.log('after 2 second print it out');
    cb(a + b)
  }, 2000)
}

add((1, 3, sum) => {
  console.log(sum);
})

```
callback的fn里 会被另外一个调用 看第2 code
有cb fn的 最后的值会储存进cb里，让后在另外一个fb可以调用他

``` javascript
const request = require('request')
//geocoding - 地理编码
const geocode = (address, callback) => {
//   mapbox - geocoding page
    const url = '';
    request({ url, json: true }, (error, { body }) => {

        const { place_name: location, center } = body.features[0];

        // cb在这里 
        if (error) {
            callback('Unable to connect to location services!', undefined)
        } else if (body.message || body.features.length === 0) {
            callback('Unable to find location. Try another search.', undefined)
        } else {
            callback(undefined, {
                latitude: center[1],
                longitude: center[0],
                location,
            })
        }
    })
}

module.exports = geocode
```
#### 
``` javascript
                                              这些都是上面设置cb的data
                                            v         v        v
    geocode(req.query.address, (error, {latitude, longitude, location} = {}) => {
        if(error) {
            return res.send({ error})
        }
        //给 另外一个api使用
        forecast(latitude, longitude, (error, forecastData) => {
            if(error){
                return res.send({ error})
            }
            res.send({
                forecast: forecastData,
                location,
                address: req.query.address
            })
        })

    })
})
```
