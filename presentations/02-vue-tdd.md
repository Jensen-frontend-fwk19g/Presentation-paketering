<!-- .slide: class="intro" -->
# Paketering, leverans och uppföljning
## TDD i Vue
###### david.andersson@zocom.se

Note: tryck S för att öppna speaker view

---
## Idag

* Enhetstester i Vue


---
## Testning i Vue
### Två approacher
1. Testa användargränssnittet - ser användaren rätt saker? <!-- .element: class="fragment" -->

2. Testa datamodellen - har komponenten rätt värden? <!-- .element: class="fragment" -->

---
## Exempel, on/off button
*Skapa en komponent med ett button-element med texten "on". När man klickar på det ska texten växla från "on" till "off" och tillbaka.*


Kravspec: <!-- .element: class="fragment" -->
+ när komponenten visas ska texten vara "on" <!-- .element: class="fragment" -->
+ när användaren klickar och texten är "on" ska den byta till "off" <!-- .element: class="fragment" -->
+ när användaren klickar och texten är "off" ska den byta till "on" <!-- .element: class="fragment" -->

---
## Exempel, on/off testkod
```javascript[1-13|1-2|4|5-6|8-10|12]
import { shallowMount } from '@vue/test-utils'
import OnOff from './OnOff.vue'

it('should show "on" when component is shown', () => {
	const expected = 'On';
	let wrapper = shallowMount(OnOff);

	let button = wrapper.find('button');  // CSS selector
	let actual = button.text();
	expect(actual).toBe(expected);
})
```

---
## Exempel, on/off komponent
```html
<template>
	<button @click="handleClick">{{isOn ? 'On' : 'Off'}}</button>
</template>
```
```javascript
export default {
	data: function() {
		return { isOn: true }
	},
	methods: {
		handleClick() { this.isOn = !this.isOn; }
	}
}
```

---
## Exempel, events
```javascript[1|2-3|5-6|8-9|1-10]
it('should show "off" when button is clicked', async () => {
	const expected = 'Off';
	let wrapper = shallowMount(OnOff);

	let button = wrapper.find('button');  // CSS selector
	await button.trigger('click')

	let actual = button.text();
	expect(actual).toBe(expected);
})
```


---
## Sammanfattning
<ul>
	<li> Använd <code>@vue/test-utils</code> för att testa Vue-komponenter </li><!-- .element: class="fragment" -->
	<li> Testa ur användarens perspektiv!<!-- .element: class="fragment" -->
	<li>Ser användaren rätt innehåll? (text osv.)<!-- .element: class="fragment" -->
	<li>Händer rätt saker när anv. interagerar med komponenten?<!-- .element: class="fragment" -->
</ul>


Note: Vue Testing Library rekommenderas i stället för Vue Test Utils, men den förra innehåller en bug i senaste versionen (2020-08) och kan inte användas med events.

---
<!-- .slide: class="code" -->

## Lets code!
Vi arbetar med övningarna, stycke 2.
