

# L09 - La directive `v-model` 

## Définition

La directive `v-model` permet de créer une liaison dans les deux sens (entre partie `template` et partie `script`.

Elle peut être utilisée sur :

- les `input`
- les `select`
- les `textarea`
- les composants

## Objectif

Synchroniser les valeurs de plusieurs champs `input`.

## Étape 1/1

### App.script

Commençons par créer une variable primitive réactive :

```javascript
import { ref } from "vue";
let message = ref("");
```

### App.template

Ensuite il suffit d'accrocher la variable `message` au `input` avec la directive `v-model` et de copier la ligne de `html` plusieurs fois pour observer la simplicité de synchronisation des données bi-directionnelle avec cette directive :

```html
<p><input v-model="message" /></p>
<p><input v-model="message" /></p>
<p><input v-model="message" /></p>
<p><input v-model="message" /></p>
<p><input v-model="message" /></p>
```

----

# Défi

Programmer un utilitaire qui demande à l'utilisateur une température en Celsius et affiche le résultat en faraneight et **vis-versa** et qui reset à zéro la température en celcius avec un bouton.

---

