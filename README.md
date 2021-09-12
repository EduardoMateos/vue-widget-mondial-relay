PACKAGE IN DEVELOPMENT, it works but needs improvement ;)

![Captura de pantalla 2021-09-11 a las 17 09 04](https://user-images.githubusercontent.com/11529050/132952455-2d93140a-812b-45b1-bb68-a164cc96fea1.png)

# Vue Widget Mondial Relay

_Mondial Relay Widget https://widget.mondialrelay.com/parcelshop-picker/v4_0/codesamples/Sample-LightImplementation.aspx to Vue_

## Installation

### Using npm

`npm i --save vue-widget-mondial-relay`

## Props API

| Props                     | Type            | Required | Default             | Details                                          |
|---------------------------|-----------------|----------|---------------------|--------------------------------------------------|
| brand                     | String          | true     |                     | Provider by Mondial Relay                        |
| defaultPostCode           | String          | false    | 59000               | Default postal Code used for search at loading   |
| defaultCountry            | String          | false    | FR                  | FR, ES, BE, NL, LU, DE, AT                       |            
| maxResults                | Int             | false    | 7                   | Must be less than 20                             |
| deliveryMode              | String          | false    | 24R                 | Standard [24R], XL [24L], XXL [24X], Drive [DRI] |

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
