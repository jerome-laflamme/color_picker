

# L08.2 Les ``computed`` avec `get` et `set`

## Définition

L'objectif ici c'est de permettre à l'utilisateur de créer un nom complet à l'aide d'un prénom et d'un nom et cela de façon réversible; extraire le prénom et le nom à partir d'une nom complet.

## Étape 1/3

### App.script

Nous débutons par la création de deux variables réactives :

```javascript
import { ref } from "vue";

const firstname = ref("Francis");
const lastname = ref("Lebeau");
const fullname = `${firstname.value} ${lastname.value}`
```

### App.template

Dans le template, nous voulons des champs d'entrée de données afin d'être en mesure d'entrer et de recevoir un prénom et un nom et d'afficher le nom complet que nous coderons dans la prochaine étape.

```html
<p>Nom complet: {{ fullname }}</p>
<p>Prénom: {{ firstname }}</p>
<p>Nom: {{ lastname }}</p>
```

Constatez que le nom complet s'affiche totalement

## Étape 2/3

### App.template

Au tout début du template, ajoutez des `input` qui seront responsable de l'entrée de données.

```html
<!--> Ajout de ce code <-->
<h3>Utilisation du get</h3>
<label>Prénom: </label>
<input type="text" @input="firstname = ($event.target as HTMLInputElement).value" />
<label> Nom: </label>
<input type="text" @input="lastname = ($event.target as HTMLInputElement).value" />
 

<p>Nom complet: {{ fullname }}</p>
<p>Prénom: {{ firstname }}</p>
<p>Nom: {{ lastname }}</p>
```

### App.script

Ajoutons maintenant le `computed` qui sera responsable de l'affichage, utlisant de la cache, du nom complet de la personne et supprimez la variable régulière `fullname`.

```javascript
const fullname = computed(() => `${firstname.value} ${lastname.value}`);
```

Remarquez que tout fonctionne comme avant.  

Le `get()` permet d'indiquer quel opérations nous voulons exécuter lorsque nous tentons d'**obtenir** la valeur de `fullname`.

Modifier le code en conséquence :

```javascript
const fullname = computed({
  get() {
    return firstname.value + " " + lastname.value;
  },
  set(newValue: string) {
    console.log(newValue);
  },
});
```

Ici, si vous tentez d'affecter une nouvelle valeur à `fullname` vous constaterez que cette dernière s'affiche dans la console.

## Étape 3/3

### App.template

À l'inverse du résultat de l'étape précédente, nous souhaiterons également **extraire** le prénom et le nom d'un nom complet.  Pour ce faire nous créerons d'abord un champ d'entrée pour le nom complet.  Placez ce code entre les 

```html
<h3>Utilisation du get</h3>
<input type="text" @input="firstname = ($event.target as HTMLInputElement).value" />
<input type="text" @input="lastname = ($event.target as HTMLInputElement).value" />

<p>Nom complet: {{ fullname }}</p>

<!--> Ajout de ce code <-->
<h3>Utilisation du set</h3>
<label> Nom complet: </label>
<input type="text" @input="fullname = ($event.target as HTMLInputElement).value" />

<p>Prénom: {{ firstname }}</p>
<p>Nom: {{ lastname }}</p>
```

### App.script

Voici comment s'appliquera le `set()` :

```javascript
const fullname = computed({
  get() {
    return firstname.value + " " + lastname.value;
  },
  set(newValue: string) {
    [firstname.value, lastname.value] = newValue.split(" ");
  },
})
```

Le prénom et le nom sont extraits de newValue grâce à la fonction `split()` .

---

# Exercice

Créez une application qui permet d'entrer une date dans une variable `birthdate` mais qui demande la [confirmation](https://www.w3schools.com/jsref/met_win_confirm.asp) de l'utilisateur avant de réellement changer la valeur dans la variable.