<!-- .slide: class="intro" -->
# Paketering, leverans och uppföljning
## TDD i Vue - mocking
###### david.andersson@zocom.se

Note: tryck S för att öppna speaker view

---
<!-- .slide: class="complex pink" -->
## Idag

* describe
* fejka props &amp; data
* stubs
* Mocking - AJAX


---
<!-- .slide: class="basic blue" -->
## Organisera testfall
Vi kan gruppera testfall som hör ihop under en rubrik, med funktionen *describe*.

```javascript[1]
describe('MyComponent.vue', () => {
	it('should (test case here)', async () => {
		// your test case here
	})

	it('should (another test case)', async () => {
		// your test case here
	})
})
```


---
<!-- .slide: class="basic blue" -->
## Mounting with props and data
Vi kan simulera att en komponent får *props* genom att skicka med ett objekt till `mount` eller `shallowMount`:
```javascript[2-4|5-7|]
const wrapper = shallowMount(MyComponent, {
	props: {
		fromParent: 'Exempel props'
	},
	data: {
		value: 42
	}
});
```

```html
<template>
	<div> Example: props={{ fromParent }}, data={{ value }} </div>
</template>
```


---
<!-- .slide: class="basic blue" -->
## Component stub
`shallowMount` renderar en komponent och ersätter eventuella *child components* (inre komp.) med *stubs*. I stället för riktiga komponenter får vi tomma komponenter, som renderas som tomma strängar.

```javascript[1|2-3]
const wrapper = shallowMount(MyComponent).findComponent(Child1);
wrapper.exists() === true
wrapper.text() === ''
```

`mount` renderar alla *child components* utom de som vi väljer att stubba:
```javascript[|1,3,7|1,4,8]
const wrapper = mount(MyComponent, {
	stubs: {
		Child1: true,  // empty component
		Child2: '<div> Use this string instead </div>'
	}
})
wrapper.findComponent(Child1).text() === ''
wrapper.findComponent(Child2).text() === '&lt;div...'
```


---
<!-- .slide: class="basic blue" -->
## Mocking 1/3
Tänk dig en komponent som gör AJAX när den renderas. När man testar den, vill man inte att den ska göra AJAX på riktigt! Det gäller att byta ut funktionen mot en egen.

npm-paketet `jest-fetch-mock` innehåller funktioner för att göra det enkelt att testa att ett fetch-anrop äger rum.

```html
&lt;template>
	&lt;button @click="handleClick"> Fetch &lt;/button>
	&lt;div>{{ ajaxData }}</div>
&lt;/template>
```

```javascript
methods: {
	async handleClick() {
		const response = await fetch('https://example.com');
		const data = await response.json();
	}
}
```


---
<!-- .slide: class="basic blue" -->
## Mocking 2/3
Dela upp i två testfall:
1. testa *att* fetch anropas

```javascript
import { enableFetchMocks } from  'jest-fetch-mock'

it('should call fetch when clicked', () => {
	enableFetchMocks();
	const result = 'Exempel';
	fetch.mockResponseOnce(JSON.stringify(result))

	const wrapper = shallowMount(MyComponent);
	await wrapper.find('button').trigger('click');

	let numCalls = fetch.mock.calls.length;
	expect(numCalls).toBe(1);
})
```

Läs mer: https://github.com/jefflau/jest-fetch-mock#readme


---
<!-- .slide: class="basic blue" -->
## Mocking 3/3
Dela upp i två testfall: <br>
2. testa att rätt värden *visas*

```javascript
it('should show AJAX results correctly', () => {
	const result = 'Exempel';

	const wrapper = shallowMount(MyComponent);
	wrapper.setData({ ajaxData: result })

	let displayedText = wrapper.find('.result').text();
	expect(displayedText).toBe(result);
})
```


---
<!-- .slide: class="code" -->

## Lets code!
Vi arbetar med övningarna.
