# Guide TP : Introduction à FastAPI

Ce guide accompagne le notebook `TP_introduction_FastAPI.ipynb`. Votre objectif est de comprendre les bases de FastAPI en complétant les parties de code manquantes et en répondant aux questions de compréhension.

Dans ce TP on va utiliser des données d'items sur une MarketPlace (laptop, souris, ..), leur prix et descriptions comme données que notre API va servir. Par souci de simplicité, on va utiliser `items_db` qui est un dictionnaire python comme "Base de Données" à la place d'une Relational Database traditionnel comme PostgRe ou MySQL.

`items_db` va être mise à jour à chaque requete PUT, DELETE, POST. 

## Objectifs Pédagogiques
- Comprendre la structure d'une application FastAPI.
- Créer des routes API (GET, POST, PUT, DELETE).
- Utiliser la validation de données avec Pydantic.
- Manipuler les paramètres de chemin (Path) et de requête (Query).
- Comprendre la différence entre sync et async.
---

## Concepts Clés : Les Décorateurs

Avant de commencer, il est important de comprendre ce qu'est un **décorateur** en Python.

- **En Python**, un décorateur est une fonction qui prend une autre fonction (ou classe) en entrée et étend son comportement sans modifier son code source explicitement. On les reconnaît au symbole `@` placé au-dessus d'une définition de fonction.
- **Dans FastAPI**, `@app.get("/")` (et ses amis `@app.post`, etc.) est un décorateur qui indique à FastAPI que la fonction python définie juste en dessous correspond au chemin (path) `/` et à l'opération HTTP `GET`.
    - Il **enregistre** cette fonction comme le gestionnaire (handler) pour cette route spécifique.
    - Il s'occupe de la sérialisation des données et de l'exécution de la fonction quand une requête arrive.

---

## Partie 1 : Création de la première application (First App)

**Contexte** : Une application FastAPI commence toujours par une instance de la classe `FastAPI`.

**Tâche 1** : Dans la section **2. Creating Your First FastAPI App**, complétez la fonction `read_root`.
- La fonction doit retourner un dictionnaire JSON simple (ex: `{"message": "Hello World"}`).

**Question 1** :  Aller sur le localhost sur lequel l'api est lancée. Que voyez vous comme message? 
**Question 2** : À quelle URL pouvez-vous consulter la documentation automatique (Swagger UI) une fois le serveur lancé ? Et la documentation plus user friendly comment s'appelle-t-elle ? Aller voir les deux pages de doc et observerz l'interface utilisateur de la doc FastAPI.

---

## Partie 2 : Méthodes HTTP (GET)

**Contexte** :  La méthode GET est utilisée pour récupérer des données.

**Tâche 2** : Dans la section **3. Understanding HTTP Methods**, complétez la fonction `get_item`.
- Vérifiez si l'`item_id` existe dans `items_db`.
- Si non, levez une `HTTPException` avec le code 404.
- Si oui, retournez l'objet correspondant.

---

## Partie 3 : Modèles Pydantic 

**Contexte** : Pydantic permet de valider les données entrantes. POST est utilisé pour créer des ressources.

**Question 2** : Quel est l'avantage d'utiliser un modèle Pydantic comme `Item` en paramètre de la fonction plutôt qu'un simple dictionnaire ?

**Question 3** : Chercher sur la doc de pydantic comment tranformer une instance de pydantic en dictionnaire. 

**Question 4** : Essayez de créer une instance de  la classe `Item` sans respecter les types des champs. Quelle erreur est affichée?

## Partie 4: La méthode POST

**Tâche 3** : Dans la section **4. Understanding HTTP Methods : POST**, complétez la fonction `create_item`.
- Générez un nouvel ID (auto-incrément).
- Ajoutez le nouvel item dans `items_db`.
- Retournez l'objet créé en respectant le `response_model`.

**Tâche de vérification** :
1. Allez sur la documentation interactive (Swagger UI) à l'URL `/docs`.
2. Utilisez le bouton "Try it out" sur votre endpoint `POST /items/` pour envoyer une requête.
3. Remplissez le corps de la requête avec un nouvel item.
4. Revenez dans le notebook et affichez le contenu de `items_db` pour remarquer qu'il a bien été mis à jour avec votre nouvel item.

---

## Partie 5 et 6 : Mise à jour et Suppression (PUT & DELETE)

**Contexte** : PUT remplace/met à jour une ressource, DELETE la supprime.

**Tâche 4** : Dans la section **5. Understanding HTTP Methods : PUT** et **## 6. Understanding HTTP Methods : DELETE** , complétez `update_item` et `delete_item`.
- Pour `update_item` : Mettez à jour les champs de l'objet existant avec les nouvelles valeurs fournies. Gérer le cas où l'objet n'existe pas.
- Pour `delete_item` : Supprimez l'objet du dictionnaire `items_db` et retournez un message de confirmation.

**Tâche de vérification** :
1. Allez sur la documentation interactive.
2. Testez vos endpoints `PUT` et `DELETE` via le bouton "Try it out".
3. Vérifiez à chaque fois dans le notebook que l'état de `items_db` a changé (item modifié ou supprimé).

---

## Partie 7 : Paramètres de Requête (Query Parameters)

**Contexte** : Les paramètres de requête ("?q=...") sont souvent utilisés pour le filtrage ou la recherche.

**Tâche 5** : Dans la section sur les paramètres de requête, complétez la logique de filtrage dans `search_items`.
- Si `q` est fourni, filtrez les items dont le nom contient la chaîne `q`.
- Si `min_price` est fourni, ne gardez que les items dont le prix est supérieur.

**Question 3** : Quelle est la différence fondamentale entre un paramètre de chemin (ex: `/items/{item_id}`) et un paramètre de requête (ex: `/items?q=abc`) ?

---

## Partie 8 : Asynchone vs Synchrone

**Contexte**: FastAPI est connue pour supportée le ASGI donc le mode asynchrone

**Tache**: Explorer le code synchrone vs asynchrone

**Question** : Quelles sont les mots clés d'une endpoint asynchrone ? A quoi ils servent ?


## Exercices Avancés (Bonus)

Si vous avez terminé le notebook :
1. Ajoutez un endpoint `/health` qui retourne `{"status": "ok"}`.
2. Ajoutez un champ `category` au modèle `Item` et essayez de créer un nouvel objet avec ce champ.

## Vérification

À la fin du notebook, Try all the endpoints.
