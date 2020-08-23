<!-- .slide: class="intro" -->
# Paketering, leverans och uppföljning
## TDD i Vue - events
###### david.andersson@zocom.se

Note: tryck S för att öppna speaker view

---
<!-- .slide: class="complex pink" -->
## Idag

* Forts. enhetstester i Vue

* https://vue-test-utils.vuejs.org/guides


---
## Sammanfattning
<ul>
	<li> Använd <code>@vue/test-utils</code> för att testa Vue-komponenter </li>
	<li> Testa ur användarens perspektiv!
	<li>Ser användaren rätt innehåll? (text osv.)
	<li>Händer rätt saker när anv. interagerar med komponenten?
</ul>

Note: Vue Testing Library rekommenderas i stället för Vue Test Utils, men den förra innehåller en bug i senaste versionen (2020-08) och kan inte användas med events.


---
<!-- .slide: class="basic blue" -->
## Shallow mounting
> *Mount a component without rendering its child components*

```javascript
const wrapper = shallowMount(MyComponent);
```

För att testa komponenten fristående från alla andra komponenter.


---
<!-- .slide: class="basic blue" -->
## Plocka ut element ur template
Exempel: testa att rätt meddelande skrivs ut i komponenten.

```html[]
<!-- Template -->
<div class="component">
	<span class="message"> {{ message }} </span>
</div>
```

```javascript[]
// find() uses CSS selector syntax, like querySelector
let span = wrapper.find('div .message');

// text() returns the text contents for any wrapper object
let message = span.text();
```


---
<!-- .slide: class="basic blue" -->
## Simulera event
Exempel: testa att rätt sak händer när vi klickar på en button.

```html[]
<!-- When the button is clicked, the "timer" data property
should be set to 0 -->
<div class="component">
	<button> Reset timer </button>
</div>
```

```javascript[]
// Find the button and simulate a click on it
let resetButton = wrapper.find('button');
await resetButton.trigger('click');

// Get the data property
// vm works with methods and computed properties too
let timer = wrapper.vm.timer;
```
Note: vm är read only


---
<!-- .slide: class="basic blue" -->
## Ändra Vue-objektet
När man testar behöver man ibland manipulera själva data-objektet.

```html[]
&lt;div>
	<div class="display">{{ message }}
</div>
```

```javascript[]
// Default component values
let textBefore = wrapper.find('.display').text();

// Change values in order to setup our tests
wrapper.setData({ message: 'changed' });
let textAfter = wrapper.find('.display').text();
```


---
<!-- .slide: class="basic pink" -->
## Vue repetition
#### Data från parent till child components
Vue skickar data till child components med *v-bind*.

```html
<!-- Parent skickar värde från template -->
<template>
	<child-component :property="value" />
</template>
```

```javascript
// Child component tar emot värde i Vue-objektet
export default {
	props: {
		property: String('default value')
	}
}
```
Note: repetition av Vue


---
<!-- .slide: class="basic pink" -->
## Vue repetition
#### Data tillbaka från child till parent component
Vue skickar data tillbaka till parent component i ett event med *$emit*.

```javascript
// Child component skickar event från funktioner i Vue-objektet
export default {
	methods: {
		someFunction() {
			this.$emit('event-name', 'Event data');
		}
	}
}
```

```html
<!-- Parent template tar emot event -->
<template>
	<child-component @event-name="handleEvent" />
</template>
```


---
<!-- .slide: class="basic blue" -->
## Är ett element synligt?
### v-if
Om villkoret är false, tar bort elementet ur DOM. <br>Kontrollera att `wrapper.find !== null`

```html
<div v-if="false"> Jag finns inte </div>
```
```javascript
let element = wrapper.find('div');
expect(element.exists()).toBe(false);
```


---
<!-- .slide: class="basic blue" -->
## Är ett element synligt?
### v-show
Om villkoret är false, sätter `display: none`<br>Kontrollera elementets `display`-egenskap

```html
<div v-show="false"> Jag är osynlig </div>
```
```javascript
let element = wrapper.find('div');
let isVisible = element.style.display !== 'none';
expect(isVisible).toBe(false);
```


---
<!-- .slide: class="basic blue" -->
## Skicka information med events
När användaren klickar på ett element behövs ingen mer information. Men när användaren skriver i ett input-fält så måste man skicka med mer information.

```javascript[1-2|4-5|7-9]
// Simulerar tryck på tangenten "a"
await wrapper.trigger('keydown', { key: 'a' })

// Tryck på enter
await wrapper.trigger('keydown.enter')

// Ändra texten i ett input-fält
// Triggar automatiskt change-händelsen och v-model
await wrapper.setValue('newValue')
```

---
<!-- .slide: class="basic blue" -->
## Egna events
Trigga ett eget event med `vm.$emit`

```javascript
await wrapper.vm.$emit('my-event', 'Data for my event');
```

---
<!-- .slide: class="code" -->

## Lets code!
Vi arbetar med övningarna.
