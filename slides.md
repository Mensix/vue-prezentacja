---
colorSchema: light
lineNumbers: true
---

# Vue

Projekt zespołowy
---


# HTML na początek

```html
<!-- Reszta tagów pominięta -->
<script>
let count = 0;

function onclick(e) { // e: MouseEvent
  count++;
  e.target.textContent = "Licznik: " + count // e.target: HTMLButtonElement
}
</script>

<button onclick="onclick()">
Licznik: 0
</button>
```
---

# Czym jest Vue?

* Vue to **framework** JavaScript stworzony na podstawie HTML, CSS i JavaScript. Jest wykorzystywany głównie do tworzenia stron i aplikacji internetowych.
* *Framework - narzędzie, które pomaga w tworzeniu aplikacji, dostarczając gotowych rozwiązań, które można wykorzystać w projekcie.*
---

# Przykładowy kod

```vue
<script setup lang="ts">
import { ref } from 'vue'

const count = ref(0)
</script>

<template>
  <button @click="count++">
    Licznik: {{ count }}
  </button>
</template>

<style>
button {
  /* ... */
}
</style>
```

<Counter />
---

# O co w tym kodzie chodzi...

* Vue posiada swoje rozszerzenie plików .vue, w którym tworzone są jego komponenty (Single File Components)
* *Komponent - "kawałek" kodu zawierającego definicje elementu, który będzie potem (wielokrotnie) wykorzystywany, działają w nim wszystkie HTMLowe tagi, na przykład button. Komponenty stosuje się do organizacji kodu.*
* Każdy SFC **musi** co najmniej mieć blok ```template```, w którym definiujemy kod podobny do tego w HTMLu.
* Może mieć również blok ```script```, w którym piszemy kod w JavaScript lub TypeScript, definiujący zachowanie komponentu
* Oraz blok ```style```, w którym definiujemy styl elementów zawartych w bloku ```template```
* Koncepcja nakazuje kolejność bloków:
    * 1. ```script```
    * 2. ```template```
    * 3. ```style```
---

# template

* Każdy SFC **musi** co najmniej mieć blok ```template```, w którym definiujemy kod podobny do tego w HTMLu
* W bloku ```template``` możemy "wstawiać" wartości zmiennych zdefiniowanych w bloku ```script``` za pomocą składni "mustache", czyli za pomocą podwójnych klamr
* Eventy definiujemy wstawiając ```@``` i nazwę naszego eventu, a w nim możemy podać funkcje zdefiniowaną w ```script``` lub jeżeli funkcja nie jest rozbudowana, bezpośrednio w cudzysłowach

```vue{5,9|5,7,9|5-9|1-9}
<script setup lang="ts">
const count = ref(0)
</script>

<template>
  <button @click="count++">
    Licznik: {{ count }}
  </button>
</template>
```
---

# HTML a template

* Każdy kod ```template``` jest poprawnym kodem HTML
* Różnica tkwi natomiast w szczegółach, Vue rozszerza standardowy HTML o swoje dyrektywy
    * interpolacja zmiennych
```vue
<script setup lang="ts">
const number = ref(0)
</script>

<template>
  <p>{{ number }}</p> 
</template>
```

<BasicCounter />
---

# Dyrektywy cd.


* pętle za pomocą dyrektywy v-for
  * *Aby Vue mógł wydajnie wyświetlać zmiany, ```v-for``` musi mieć zmienny atrybut ```key```, na przykład indeks pętli*
```vue
<template>
  <p v-for="i in 5" :key="i">{{ i }}</p>
</template>
```

<BasicFor />
---

# Dyrektywy cd.

* wyświetlanie warunkowe za pomocą v-show i v-for
  * v-show ustawiony na false renderuje element, ale ustawia styl na ```display: none;```, przez co jest on niewidoczny
  * v-if ustawiony na false w ogóle nie renderuje elementu, dodatkowo istnieją dyrektywy v-else-if i v-else działające tak samo jak można się domyśleć
  

```vue
<script setup lang="ts">
const number = ref(0)
</script>

<template>
  <p v-if="number > 0">Liczba większa niż zero!</p>
  <p v-else-if="number < 0">Liczba mniejsza niż zero!</p>
  <p v-else>Liczba równa zero!</p>
</template>
```

<BasicIf />
---

# Dyrektywy cd.

<!-- v-model -->
* v-model
  * Dyrektywa v-model jest używana do tworzenia dwukierunkowego wiązania danych (ang. two-way value binding), co oznacza, że zmiana wartości w polu formularza zmienia wartość zmiennej, a zmiana wartości zmiennej zmienia wartość pola formularza
  * Dyrektywa v-model działa zarówno na elementach input, select, textarea, jak i na komponentach stworzonych przez użytkownika

```vue
<script setup lang="ts">
const name = ref('')
</script>

<template>
  <input v-model="name" />
  <p>{{ name }}</p>
</template>
```

<BasicModel />
---

# Komponenty

* Załóżmy, że w katalogu ```/components``` mamy nastepujący plik ```me.vue```

```vue
<script setup lang="ts">
defineProps<{
  name: string
}>();

defineEmits<{
  (e: 'hello'): void
}>();
</script>

<template>
  <p @click="$emit('hello', name)">{{ name }}</p>
</template>
```
---

# Komponenty

* Wówczas to w dowolnym innym pliku .vue możemy użyć tego komponentu

```vue
<script setup lang="ts">
import Me from './components/me.vue'

function handleHello(name: string) {
  console.log('Użytkownik kliknął w imię! ' + name);
}
</script>

<template>
  <Me name="Grzegorz" @hello="handleHello" />
</template>
```

Grzegorz
---

# Słowo o komponentach

* W poprzednim kodzie pojawiło się kilka nowych rzeczy
    * Za pomocą makra ```defineProps``` definiujemy atrybuty (ang. props) naszego komponentu i jego typ
    * *Atrybuty mogą również przyjmować zmienne za pomocą wstawienia dwukropka przed jego nazwą w template*

```ts
defineProps<{
  name: string // tylko string lub funkcja zwracająca string jest dozwolona
  nazwisko?: string // ? mówi, że prop nie jest wymagany
}>()
```

  * A za pomocą makra ```defineEmits``` tworzymy własne eventy komponentu, opcjonalnie jego argumenty i zwracany typ
    * W bloku ```script``` możemy wywoływać wywołanie eventa, przypisując zmienną do wartości emitów i wywołując ją poprzez tą zmienną, mając na uwadze sygnaturę
    * Natomiast w bloku ```template``` w tym celu używamy dostępnej w nim funkcji ```$emit```

```ts
const emits = defineEmits<{
  (e: 'hello', name: string): void // pod e nazywamy nasz event, a po nim podajemy argumenty
}>()

emits('hello', 'Grzegorz')
```
---

# Zanim script

* Podstawą działania Vue jest *reaktywność*
* *Reaktywność - mechanizm umożliwiający reagowanie na zmiany wartości danych (na przykład zmiennych) bez konieczności angażowania w to użytkownika z poziomu kodu*
* Vue posiada swój własny system reaktywności, który pozwala na automatyczne aktualizowanie widoku w zależności od zmian w danych
---

# script

* Blok script jest miejscem, w którym definiujemy zachowanie naszego komponentu
* *Obecnie wskazaną konwencją używania bloku ```script``` jest definiowanie go za pomocą ```<script setup lang="ts">```, co pozwala na kolejno makr kompilatora i korzystanie z TypeScript*
* W bloku ```script``` możemy definiować zmienne, które będą reaktywne, za pomocą funkcji ```ref```, ```reactive``` lub ```computed```
    * *ref - funkcja, która zwraca obiekt, który zawiera wartość, która może być reaktywna*
    * *reactive - funkcja, która zwraca obiekt, który zawiera wartości, które mogą być reaktywne*
    * *computed - funkcja, która zwraca wartość, która może być reaktywna, ale jest obliczana na podstawie innych wartości reaktywnych*
* Możemy również definiować funkcje, które mogą być wykorzystywane w bloku ```template```
---

# ref i reactive

* ```ref``` i ```reactive``` są funkcjami, które pozwalają na tworzenie reaktywnych zmiennych
* ```ref``` tworzy reaktywną zmienną, która przechowuje pojedynczą wartość
    * *Wartość ```ref``` w bloku skryptu jest dostępna pod kluczem ```value```*
* ```reactive``` tworzy reaktywny **obiekt**, który przechowuje wiele wartości

```vue{1,6|1,2,6|1-6}
<script setup lang="ts">
const count = ref(0) // number
const count2 = reactive({ counter: 0 }) // { counter: number } 

// console.log(count.value) => 0
// console.log(count2.counter) => 0
</script>
```

* Oczywiście, te wartości mogą być dowolnego typu, możemy je również używać w bloku ```template```
---

# computed

* ```computed``` jest funkcją, która zwraca wartość, która może być reaktywna, ale jest obliczana na podstawie innych wartości reaktywnych
* Pozwala to na tworzenie zmiennych, które są zależne od innych zmiennych, ale nie musimy ich ręcznie aktualizować

```vue{1,6|1,2,6|1,2,3,6|1-6}
<script setup lang="ts">
const count = ref(0)
const doubleCount = computed(() => count.value * 2)

// doubleCount.value === count.value * 2
</script>
```
---

# style

* W bloku ```style``` tworzone są style komponentów za pomocą CSSa

```vue
<!-- Reszta kodu pominięta -->
<style>
button {
  padding: 0.5rem 0.25rem;
  border: 1px solid black;
  background-color: gray
  color: black;
}
</style>
```
---

# Nuxt

* Nasz projekt od strony frontendu będzie tworzony w frameworku Nuxt, będacy frameworkiem Vue
* Dostarcza cały ekosystem Vue w jednym miejscu, co pozwala na szybkie tworzenie aplikacji
* *Ekosystem - zbiór narzędzi, bibliotek, frameworków, które są ze sobą kompatybilne i współpracują ze sobą*
* Nuxt dostarcza między innymi:
    * routing
    * SSR (Server Side Rendering)
    * generowanie statycznych stron
    * store
    * ...i wiele innych
---

# Routing

* Routing w Nuxt jest bardzo prosty, ponieważ jest oparty na plikach
* W katalogu ```/pages``` tworzymy pliki .vue, które będą naszymi stronami
* Nuxt automatycznie tworzy routing na podstawie tych plików
* *Routing - mechanizm, który pozwala na przechodzenie między różnymi stronami w aplikacji*
* Przykładowo, jeżeli mamy plik ```/pages/about.vue```, to nasza strona będzie dostępna pod adresem ```/about```
* Możemy również tworzyć foldery, które będą grupować nasze strony
* *Foldery w Nuxt są traktowane jako grupy, które mogą zawierać inne foldery lub pliki .vue*
* *Nuxt automatycznie tworzy routing na podstawie tych plików*
---

# SSR

* SSR (Server Side Rendering) to technika, która polega na generowaniu widoku aplikacji po stronie serwera, a nie po stronie klienta
* *Widok - to to, co widzi użytkownik, czyli to, co jest wyświetlane w przeglądarce*
* Dzięki SSR, aplikacja jest ładowana szybciej, ponieważ serwer generuje widok, a nie klient
* Nuxt automatycznie generuje SSR dla naszej aplikacji
---

# Generowanie statycznych stron

* Nuxt pozwala na generowanie statycznych stron, co oznacza, że możemy wygenerować pliki HTML, CSS i JS, które możemy wrzucić na serwer
* *Statyczne strony - strony, które są generowane raz, a następnie są dostępne dla użytkowników, bez konieczności generowania ich za każdym razem, gdy użytkownik odwiedza stronę*
* Dzięki temu, nasza aplikacja jest szybsza, ponieważ nie musi generować widoku za każdym razem, gdy użytkownik odwiedza stronę

---

# Store

* Store w Nuxt to miejsce, w którym przechowujemy dane, które są współdzielone między różnymi komponentami
* *Dane współdzielone - dane, które są dostępne dla wielu komponentów, a nie tylko dla jednego*
* Store w Nuxt jest oparty najczęściej na Pinia, który jest biblioteką do zarządzania stanem w aplikacjach Vue
---

# Middleware

* Middleware w Nuxt to funkcje, które są wywoływane przed wyrenderowaniem strony
* Dzięki middleware, możemy wykonywać różne operacje, takie jak autoryzacja, walidacja, logowanie, itp.
* Middleware w Nuxt są oparte na plikach, które są umieszczone w katalogu ```/middleware```
---

# Tyle!

* Zadanie dla Was: zainstalujcie Nuxt i spróbujcie stworzyć TODO listę
* *Nuxt można uruchomić na stronie https://nuxt.new/s/v3 lub zainstalować lokalnie za pomocą ```npx create-nuxt-app```, uprzednio należy zainstalować NodeJS*
* Do tego mogą być potrzebne: reactive, v-model; input, button w formularzu

<TodoList />