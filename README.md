# vue-moments-ago
A Vue.js component for updating human readable time format.

### Install 
```
npm install vue-moments-ago
```

and in your component file script tag,

```
import VueMomentsAgo from 'vue-moments-ago'

export default{
  components: {
    VueMomentsAgo
  }
}
```

in the template, use: 

```
<vue-moments-ago prefix="posted" suffix="ago" date="2018-05-02T20:22:22.285Z"></vue-moments-ago>
```

result:
```
 posted 2 minutes ago
```

### Props

| Props        | Required     | Type            | Definition                                            |
| :----------- |:-------------| :-------------  |:-------------                                         |
| prefix       | false        | String          | Any prefix string you want to add to the output       |
| suffix       | false        | String          | Any suffix string you want to add to the output       |
| date         | true         | String          | The date is in ISO 8601 (default format of moment.js) |