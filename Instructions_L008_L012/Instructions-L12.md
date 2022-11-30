# L11 - La fonction `watchEffect()`

## Définition

### Différences entre `watch` et `watchEffect()`

**La fonction `watch()`** : n'enregistre en sources (ou dépendances) que celles explicitement passées en premier argument. Elle n'enregistre pas automatiquement les propriétés réactives utilisées dans la fonction de rappel. La fonction de rappel ne s'exécute que si au moins une source change.

**La fonction `watchEffect()`** : enregistre automatiquement toutes les propriétés réactives, utilisées dans la fonction de rappel passée en premier argument, comme des dépendances. La fonction de rappel est exécutée une première fois dès le chargement puis à chaque fois qu'une des propriétés réactives utilisées dans la fonction de rappel sont modifiées.

## Objectif

Être en mesure de réagir dès qu'une variable réactive contenue dans un code donné se met à jour.

## Étape 1

### App.script

Nous ajouterons le `watchEffect()` qui fixera le prix de vente de l'item en conséquence du coût d'achat ainsi que les dépendances nécessaires et finalement une interface pour le typage d'un `Item`.

La variable `item` représentera l'implémentation de l'interface `Item`.

```typescript
import { watchEffect, reactive, watch } from 'vue'

interface Item {
    type: string
    cost: number
    price: number
    updated_at?: string
}

const item = reactive<Item>({
    type: 'Headset',
    price: 0,
    cost: 109.99
})

watchEffect(() => {
    item.price = item.cost.value * 1.45
})
```

### App.template

Nous aurons allons créer un mini "dashboard" des ventes !

```html
<label>Coût de l'article : </label> <input v-model="item.cost" type="text" /><br /><br />
<label>Prix de vente (45% de profit) : </label>
<input v-model="item.price" type="text" disabled /><br /><br />
<label>Dernière modification : </label>
<input v-model="item.updated_at" type="text" disabled /><br /><br />
```

Notez toute la grâce des changement de ligne avec les <br/>

Observez que le prix de vente se met maintenant à jour en utilisant un `watchEffect()` qui sera calculé dès le début de l'ouverture de la page et non seulement lors de la première mise-à-jour du coût d'achat. La dernière modification ne sera pas quant-à elle mise-à-jour car, dans les faits, il n'y a eu aucune modification alors le champs demeure vide ce qui confère un UX agréable pour l'utilisateur.

## Étape 2

### App.script

Ajoutons maintenant le `watch` régulier pour le changement de la date de mise-à-jour.

```typescript
watch(item, () => {
    item.updated_at = new Date().toLocaleString()
})
```

Observez que la date de dernière modification fonctionne comme L010 car c'est effectivement encore un `watch()`.

---

# Défi

Utilisant le WatchEffect, compléter le code précédent (instructions) pour afficher une quantité et le nombre de modifications effectuées par l'utilisateur.
