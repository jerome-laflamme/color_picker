# L10 - La fonction `watch()` 

## Définition

### Les `Watchers`

Lorsque nous avons besoin d'effectuer des actions asynchrones ou des effets de bord (`side effects`) lorsque des propriétés réactives changent, il n'est pas possible d'utiliser des propriétés calculées.

Par exemple, lorsque nous avons besoin d'effectuer une requête `HTTP` pour récupérer des données supplémentaires, ou encore lorsque nous avons besoin de modifier le `DOM`.

La fonction `watch()` permet de déclencher l'exécution d'une fonction si une ou plusieurs propriétés réactives enregistrées comme dépendances sont modifiées.

#### Les sources ou dépendances de la fonction `watch()`

Le premier argument est le ou les dépendances à enregistrer, elles sont appelées les sources de la fonction `watch()`.

Cela peut être une **seule propriété** réactive, enregistrée en dépendance :

```javascript
watch(prop, (nouvelleVal) => {
  console.log(`prop vaut ${nouvelleVal}`)
})
```

Cela peut être une fonction `getter` comme pour une propriété calculée :

```javascript
watch(
  () => prop1.value + prop2.value,
  (somme) => {
    console.log(`La somme de x + y est ${somme}`)
  }
)
```

Ou encore un tableau de plusieurs propriétés réactives :

```javascript
watch([prop1, () => prop2.value], ([nouvelleValProp1, nouvelleValProp2]) => {
  console.log(`prop1 vaut ${nouvelleValProp1} et prop2 vaut ${nouvelleValProp2}`)
})
```

## Objectif

Afficher à l'écran la quantité de modifications effectuées à une variable (object) donnée.

## Étape 1/3

### App.template

Nous avons d'abord besoin d'un `input` qui sera responsable de fixer la quantité d'article en stock :

```html
<p>Nombre de modifications : {{ item.updateCount }}</p>
<label> Quantité : </label>
<input v-model="item.quantity" type="number" />
```

### App.script

Pour ce qui est du code, commençons par ajouter les dépendances que nous aurons besoin ainsi qu'un objet `item` sur lequel nous voudrions être informé d'un changement :

```javascript
import { watch, reactive, ref } from "vue";

const item = reactive({
  type: "Headset",
  price: 109.99,
  quantity: 3,
  updateCount: 0,
});
```

Il est important de ne jamais effectuer de modification à une variable contenue dans un objet observé à l'intérieur de la fonction de callback car cela créera un boucle infinie.  Vous pouvez observer ce comportement avec ce code : 

```javascript
watch(item, (newValue, oldValue) => {
  console.log("newValue", newValue, "oldValue", oldValue);
  item.updateCount++;
});
```

J'imagine que la console de votre navigateur n'a pas trop apprécié l'expérience ou, du moins, à été passablement loquace ou que le nombre de modification est assez élevé !

## Étape 2/3

Mais si cela n'est pas possible comme cela, comment allons-nous faire ?  En fait il faut simplement observer l'élément que nous souhaitons observer.  Dans ce cas c'est la quantité d'article en stock.  Sauf que nous ne pourrons pas simplement l'observer en ajoutant `.quantity` au premier paramètre de `watch` : 

```javascript
watch(item.quantity, (newValue, oldValue) => {...
```

Cela n'est pas permis car les `watch` nécessitent des variables réactives pour fonctionner (voir présentation sur les `proxies`).  Ici, item.quantity est une variable primitive alors nous aurons besoin de ce que l'on nomme un ``getter`` qui s'écrit ainsi `() => variable`.  Modifier le code de l'étape #1 :

```javascript
watch(
  () => item.quantity,
  (newValue, oldValue) => {
    console.log("newValue", newValue, "oldValue", oldValue);
    item.updateCount++;
  }
);
```

Observez maintenant dans la console qu'un changement de quantité en stock affiche seulement un élément à la console et que le `itemCount` reçoit la bonne valeur.

## Etape 3/3

Il est également possible d'observer plusieurs changement à la fois dans un watcher.  Ajouter une variable `sku` 

###  App.script

Ajouter une variable `sku` au code :

```javascript
let sku = ref("K90873264")
```

Modifiez le watcher comme suit :

```javascript
watch([() => item.quantity, sku], (newValue, oldValue ) => {
   console.log("newValue", newValue, "oldValue", oldValue);
   item.updateCount++;
})
```

### App.template

Ajoutons maintenant la possibilité de modifier le SKU également :

```html
<p>Nombre de modifications : {{ item.updateCount }}</p>
<label>Quantité :</label>
<input v-model="item.quantity" type="number" />

<!--> Ajout de ce code <-->
<label> SKU :</label>
<input v-model="sku" type="text" />
```

Observez qu'il est possible d'obtenir le nombre de modifications autant de la quantité que du SKU.

## Complémentaire

Afin de pouvoir faire cesser un `watch`, nous créons une variable ` unwatch` depuis le retour du `watch` que nous invoquons au moment de vouloir briser le lien.

```javascript
const unwatch = watch(item, (newValue, oldValue ) => {
    console.log("newValue", newValue, "oldValue", oldValue);
    item.updateCount++;
 } ) 
 unwatch();
```

 

---

# Défi

Observer le poids et la taille d'une personne et afficher réactivement son IMC précédent et actuel (après changements)
