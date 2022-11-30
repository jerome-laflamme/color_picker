# L08 - Les ``computed`` 

## Définition

Nous avions vu qu'il était possible de mettre toute expression `JavaScript` côté `template`, par exemple :

```html
<p>Est majeur :</p>
<span>{{ personne.age >= 18 ? 'Oui' : 'Non' }}</span>
```

Cependant, dès qu'il y a de la logique incluant des données réactives il est recommandé d'utiliser des propriétés calculées (`computed properties`). 

La fonction `computed()` attend en argument une fonction de rappel qui retourne la valeur calculée.

Une propriété calculée suit automatiquement le changement de ses dépendances réactives. Cela signifie que `Vue.js` sait quelles propriétés réactives sont utilisées pour déterminer la valeur de la propriété calculée.

Lorsque l'une des valeurs des propriétés réactives changent, `Vue.js` déclenche le calcul de la propriété calculée automatiquement. Lorsqu'aucune dépendance change, `Vue.js` retourne la valeur pré-calculée qui est gardée en cache.

## Objectif

Effectuer le calcul automatique des prix de vente d'un article en utilisant les avantages de la cache de `computed.`

## Étape 1/1

### App.script

Voici le code permettant d'obtenir un calcul intelligent qui se fera seulement lorsqu'une variable impliquée est modifiée.  L'utilisation de fonctions régulières entraînerait un recalcul à tous les changements de Vue.

Pour utiliser notre exemple, il faut d'abord importer les fonctions suivantes de Vue :

```javascript
import { computed, reactive } from "vue";
```

On se crée un produit  :

```javascript
const item = reactive({
  type: "Headset",
  price: 109.99,
  quantity: 3,
});
```

Voici la méthode que tout bon programmeur ferait normalement  :

```javascript
//  Ces fonctions seront exécutés à chaque changement de variable ou à l'écran
const subTotal = () => item.price * item.quantity;
const gst = () => subTotal() * 0.05;
const qst = () => subTotal() * 0.09975;
const total = () => subTotal() + gst() + qst();
```

Voici maintenant la bonne façon de faire car ``computed`` conservera une cache des valeurs et ne recalcule le total que si le prix ou la quantité change :

```javascript
//Computed gardera une cache des valeurs et ne recalculera que si c'est
//ces valaurs qui sont demandés ET modifiés
//ATTENTION: Tous les calls de computed doivent être synchrones
const subTotalC = computed(() => item.price * item.quantity);
const gstC = computed(() => subTotalC.value * 0.05);
const qstC = computed(() => subTotalC.value * 0.09975);
const totalC = computed(() => subTotalC.value + gstC.value + qstC.value);
```

N.B. ``computed`` n'accepte pas les calls Asynchrone.

### App.template

Ajoutez ce code en pug : 

```html
  h3 Mauvaise pratique - Avec des fonctions
  li Sous-total: {{ subTotal().toFixed(2) }}
  li TPS: {{ gst().toFixed(2) }}
  li TVQ: {{ qst().toFixed(2) }}
  li Total: {{ total().toFixed(2) }}

  h3 Bonne pratique - Avec computed
  li Sous-total: {{ subTotalC.toFixed(2) }}
  li TPS: {{ gstC.toFixed(2) }}
  li TVQ: {{ qstC.toFixed(2) }}
  li Total: {{ totalC.toFixed(2) }}
```



----

# Défi

Demander la date de naissance à l'utilisateur et afficher son âge.

