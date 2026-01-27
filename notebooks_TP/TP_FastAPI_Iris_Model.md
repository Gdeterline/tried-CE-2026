# Guide TP : Servir un Modèle ML avec FastAPI 

Ce guide accompagne le notebook `TP_FastAPI_Iris_Model.ipynb`. Vous allez apprendre à transformer un modèle de Machine Learning (notre classifieur Iris) en une API web utilisable par n'importe quelle application.

## Objectifs
- Créer une structure de données stricte pour l'entrée du modèle avec **Pydantic**.
- Implémenter une route API pour la prédiction.

---

## Étape 1 : Préparation du Modèle
Dans le notebook, loader le modèle RF de scikit learn que vous avez déjà commencé dans le premier notebook.

## Étape 2 : Définir le Contrat de Données (Pydantic)

**Tâche** : Complétez la classe `IrisInput`.

**Quel champ faut-il mettre dans la classe Input qui servira à faire une inférence au modèle ?**

**Pourquoi écrire une classe pydantic?**


---

## Étape 3 : Créer l'Endpoint de Prédiction

**Tâche** : Complétez la fonction `predict_species`.

1. Choix de la Méthode HTTP : pourquoi POST ?

2. Faut-il utiliser une fonction synchrone ou asynchrone ? Pourquoi?

3. Post-processing
Le modèle retourne un chiffre (0, 1 ou 2). L'API doit être "compréhensible par un humain".
Transformez ce chiffre en nom d'espèce (`setosa`, `versicolor`, `virginica`) avant de le renvoyer.

---

## Étape 4 : Tester
Vérifiez que votre API fonctionne avec la commande `requests` fournie à la fin du notebook ou via Swagger UI (http://127.0.0.1:8000/docs).

---

## Étape 5 : Structurer son code en "Production-Ready"

Une fois que votre code fonctionne dans le notebook, il est **impératif de ne pas laisser le code en vrac** dans un notebook pour une vraie application. Nous allons restructurer le code en suivant les standards de l'ingénierie logicielle.

**Objectif** : Créer un paquet python `api` contenant proprement votre application. 
Suivez l'exemple de solution situé dans : `/TP_solution/api`.

### 1. Créer le dossier et le package
Créez un dossier nommé `api` à la racine de votre projet.
À l'intérieur, créez un fichier vide nommé `__init__.py`. 
> **Pourquoi (`__init__.py`) ?** : Ce fichier signale à Python que ce dossier doit être traité comme un **package**. Cela nous permettra de faire des imports propres comme `from api.schemas import ...`.

### 2. Le fichier `schemas.py` (La couche de Données)
Créez un fichier `api/schemas.py`.
Copiez-y vos classes Pydantic (`IrisInput`, `IrisPrediction`).
> **Pourquoi ?** : On sépare la définition des formats de données ("Schemas" ou "DTO") du reste de la logique. C'est plus propre et plus facile à maintenir.

### 3. Le fichier `iris_model.py` (La couche Métier / ML)
Créez un fichier `api/iris_model.py`.
À l'intérieur, créez une classe (par exemple `IrisModel`) qui aura la responsabilité de charger le modèle (ou l'entraîner) et de faire les prédictions.
Convention de nommage : Les fichiers sont en `snake_case` (`iris_model.py`), mais la classe à l'intérieur est en `PascalCase` (`class IrisModel:`).

Exemple de structure :
```python
class IrisModel:
    def __init__(self):
        # Charger ou entrainer le modèle ici
        pass

    def predict(self, sepal_length, ...):
        # Faire la prédiction
        pass
```

### 4. Le fichier `iris_api.py` (L'application Web / Entrée)
Créez un fichier `api/iris_api.py`.
C'est ici que vous allez instancier `app = FastAPI(...)`.
- Importez vos schémas : `from api.schemas import IrisInput`
- Importez votre logique ML : `from api.iris_model import IrisModel`
- Instanciez votre modèle une seule fois au début.
- Définissez vos routes (`@app.get`, `@app.post`) qui utiliseront l'instance du modèle.

> **Pourquoi cette séparation ?** : 
> - Si demain vous changez de framework web (ex: Flask), vous gardez `iris_model.py` intact.
> - Si vous changez de modèle ML, vous ne touchez pas à `schemas.py` ni à la définition des routes API.


### 5. Lancer votre API

Ajoutez  à la fin de votre script iris_api.py ce bout de code.
```
if __name__ == "__main__":
    import uvicorn

    # Run the application
    uvicorn.run("src.iris_api:app", host="127.0.0.1", port=8000, reload=True)

```
Ensuite lancer votre script dans le treminal et vérifier que l'API se lance bien sur votre localhost.
