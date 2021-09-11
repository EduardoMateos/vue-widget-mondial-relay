PACKAGE IN DEVELOPMENT, it works but needs improvement ;)

![Captura de pantalla 2021-09-11 a las 17 09 04](https://user-images.githubusercontent.com/11529050/132952455-2d93140a-812b-45b1-bb68-a164cc96fea1.png)

# Vue Widget Mondial Relay

_Mondial Relay Widget https://widget.mondialrelay.com/parcelshop-picker/v4_0/codesamples/Sample-LightImplementation.aspx to Vue_

## Installation

### Using npm

`npm i --save vue-widget-mondial-relay`

## Props API

| Props                     | Type            | Required | Default             |
|---------------------------|-----------------|----------|---------------------|
| brand                     | String          | true     |                     |
| defaultPostCode           | String          | false    | 59000               |
| defaultCountry            | String          | false    | FR                  |
| maxResults                | Int             | false    | 7                   |
| deliveryMode              | String          | false    | 24R                 |

## Example

```HTML
<template>
  <div id="app" style="max-width: 980px; margin: auto">
    <WidgetMondialRelay
        brand="BDTEST  "
        defaultPostCode="59000"
        defaultCountry="FR"
        maxResults="7"
        deliveryMode="24R"
        @select="setParcelSelected($event)"
    />
  </div>
</template>
```

```JS
import WidgetMondialRelay from 'vue-widget-mondial-relay';
import 'vue-widget-mondial-relay/dist/widget-mondial-relay.min.css';

export default {
  name: 'App',
  components: {
    WidgetMondialRelay
  },
  methods:{
    setParcelSelected(data){
      console.log(data)
    }
  }
}
```
