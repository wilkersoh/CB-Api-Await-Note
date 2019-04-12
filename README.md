
## CallBack
callback的fn里 会被另外一个调用 看第2 code
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
