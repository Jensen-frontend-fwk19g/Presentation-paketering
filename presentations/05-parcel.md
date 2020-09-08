<!-- .slide: class="intro" -->
# Paketering, leverans och uppföljning
## Byggverktyg
###### david.andersson@zocom.se

Note: tryck S för att öppna speaker view,
F för fullscreen,
Alt + Click för att zooma in/ut,
O för Overview mode

---
<!-- .slide: class="complex pink" -->
## Idag

+ Byggverktyg
+ Environment
+ Konfigurationsfiler

---
<!-- .slide: class="basic blue" -->
## Några byggverktyg
| Module bundlers |                    |
| :-------------- | :----------------- |
| Webpack         | All inclusive      |
| Parcel          | Zero configuration |
| Browserify      | Only bundler       |

<br>

| Task runners  |              |
| :------------ | :----------- |
| Grunt         | Aktiveras när filer ändras         |
| gulp          | Aktiveras när filer ändras (async) |
<!-- .element: class="fragment" -->

<br>

| Transpilers |               |
| :---------- | :------------ |
| Babel       | ES6+ till ES5 |
<!-- .element: class="fragment" -->

Läs: [Webpack vs Gulp vs Grunt vs Browserify](https://alligator.io/tooling/webpack-gulp-grunt-browserify/), <br>
[JavaScript transpilers, what and why](https://scotch.io/tutorials/javascript-transpilers-what-they-are-why-we-need-them)


---
<!-- .slide: class="basic blue" -->
## Installera Parcel
```bash
npm init -y
npm install parcel --save-dev
```

*Glöm inte att skapa ett git-repo med en .gitignore!*


---
<!-- .slide: class="basic blue" -->
## Package.json
Skapa ett skript som startar Parcels utvecklingsserver:
```json[3]
...
"scripts": {
	"start": "parcel serv ./src/index.html",
	...
},
...
```

Parcel kräver väldigt lite konfiguration. Det tittar på HTML-filen för att avgöra vad som ska inkluderas i projektet.

Starta utvecklingsservern: <br>
```bash
npm run start
```


---
<!-- .slide: class="basic blue" -->
## Vue 1/2
Installera Vue, utan att använda "vue cli":
```bash
npm i vue
```

Lägg till ett element i body, som Vue kan injiceras i:
```html[3]
&lt;body>
	&lt;noscript> Den här appen använder JavaScript &lt;/noscript>
	&lt;div id="app"> &lt;/div>
&lt;/body>
```


---
<!-- .slide: class="basic blue" -->
## Vue 2/2
Skapa filer: `src/index.js, src/components/App.vue`
```vue
import Vue from 'vue';
import App from './components/App';

// För att slippa en varning
Vue.config.productionTip = false

// Här renderas appen och injiceras i '#app'
new Vue({
    render: h => h(App),
}).$mount('#app')
```


---
<!-- .slide: class="basic blue" -->
## Environment variables
Problem:
1. lösenord och API-nycklar m.m. ska inte lagras i git-repot<!-- .element: class="fragment" -->
2. olika miljöer kan ha olika konfigurationer<!-- .element: class="fragment" -->

Lösning:<!-- .element: class="fragment" -->
1. skapa en .env-fil<!-- .element: class="fragment" -->
2. lägg till .env i .gitignore<!-- .element: class="fragment" -->
2. filen läses in automatiskt av Parcel<!-- .element: class="fragment" -->



---
<!-- .slide: class="basic blue" -->
## .env file
Hemliga värden lägger vi i env file
```bash
USER="admin"
PASSWORD="I234567B"
API_KEY="05960y49y9065y"
```

Använda värden i JavaScript-fil
```javascript
// Alla värden sparas i objektet process.env
console.log('.env file, USER: ', process.env.USER);

console.log(process.env);
// Skriver bara ut {}, av säkerhetsskäl
```

Obs! Lägg till `.env` i `.gitignore`!


---
<!-- .slide: class="basic blue" -->
## Environment variables i package.json
Lägg till `VARIABEL="värde"` först i skript-raden:
```package.json
"scripts": {
	"dev": "NODE_ENV=dev parcel serve ./src/index.html"
}
```

Fungerar inte på Windows! Använd paketet cross-env:
```bash
npm install cross-env --save-dev
```
```package.json
"scripts": {
	"dev": "cross-env NODE_ENV=dev parcel serve ./src/index.html"
}
```

Note: lägg till --no-cache om parcel "kommer ihåg" felaktiga värden mellan körningar.

---
<!-- .slide: class="basic blue" -->
## TDD?
| Header One   | Header Two     |
| :----------- | :------------- |
| Parcel       | **zero** config module bundler |
| Jest         | **zero** config test runner    |
| Parcel + Jest | **LOTS of config**<!-- .element: class="fragment" --> |


![](https://media.giphy.com/media/glmRyiSI3v5E4/giphy.gif)<!-- .element: class="fragment" -->


---
<!-- .slide: class="basic blue" -->
## Varför är TDD svårt?
+ För att testa en Vue-komponent behöver Jest läsa *.vue-filer
+ Jest kan inte göra det på egen hand
+ Transpilatorn **Babel** kan översätta `.vue`-filer till `.js`
+ De måste konfigureras i filerna `.babelrc` och `jest.config.js`
+ Babel kräver ett "preset" för Vue, som är svårt att hitta

<br><br>

**Enklare lösning:** Vue CLI har inbyggt stöd för både Jest och Babel!

Vi tittar närmare på: <br> https://github.com/Jensen-frontend-19/03-vue-exercises


---
<!-- .slide: class="code" -->

## Lets code!
Vi jobbar med övningarna på byggverktyg:
[länk till övningar](https://docs.google.com/document/d/1MronC9nu7B6z46g2OZ8BQ610TCAtqMjXxcyD8zvH_Fo/edit?usp=sharing)
