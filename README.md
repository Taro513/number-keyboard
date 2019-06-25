#number keybard

> a number keyboard for pc

## Build Setup

``` bash
# install dependencies
npm install simple-number-keyboard

# import dependency
import NumberKeyboard from 'NumberKeyboard'
Vue.use(NumberKeyboard);

# usage
<number-keyboard :number-length="numberLength" ref="keyboard"
 @get-val="getVal"></number-keyboard>
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).
