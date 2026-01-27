# Guide TP : Pipeline Machine Learning Complet (Iris Dataset)

Ce guide accompagne le notebook `TP_simple_ML_model.ipynb`. Votre objectif est de construire un pipeline de Machine Learning complet pour classifier les fleurs d'Iris, puis de structurer votre code proprement.

## Objectifs Pédagogiques
- Charger et explorer un dataset classique (Iris).
- Créer et entraîner un pipeline scikit-learn (Preprocessing + Modèle).
- **Refactoring** : Sortir du notebook pour créer un script Python modulaire.

---

## Étape 1 : Chargement et Exploration des Données

**Tâche** : Complétez la fonction `create_train_test_split` dans le notebook. Utilisez `train_test_split` de scikit-learn.

---

## Étape 2 : Pré-traitement des Données (Preprocessing)

**Tâche** : Complétez la fonction `create_preprocessing_pipeline` pour retourner un `StandardScaler`.

**Question** : Le `StandardScaler` transforme les données pour qu'elles aient une moyenne de 0 et un écart-type de 1. Pourquoi est-ce important pour certains algorithmes de Machine Learning d'avoir des données à la même échelle ?

---

## Étape 3 : Création du Pipeline Modèle

**Tâche** : Complétez `create_model_pipeline` pour assembler le scaler et le classifieur `RandomForestClassifier`.

**Question** : Quel est l'avantage d'utiliser un objet `Pipeline` de scikit-learn plutôt que d'appliquer manuellement le scaler puis le modèle ? (Indice : pensez à ce qui se passe lors de la prédiction sur de nouvelles données).
**Question** :  Comment est-ce que cette pipeline est utile à avoir si on va développer une API avec ce modèle ?
---

## Étape 4 : Évaluation du Modèle

**Tâche** : Complétez `evaluate_model` pour calculer la matrice de confusion, l'accuracy, la précision, le rappel (recall) et le F1-score.

---

## Étape 5 : Exécution Complète

**Tâche** : Complétez la fonction `main` pour orchestrer toutes les étapes précédentes. Exécutez le notebook et observez les graphiques résultats.

---

## 🚀 Étape Finale : "Clean Code" & Refactoring

Les notebooks sont excellents pour l'expérimentation, mais pour mettre un modèle en production ou travailler en équipe, nous utilisons des scripts Python `.py`.

**Votre mission :**

1.  Créez un nouveau fichier nommé `model_pipeline.py` dans le dossier `src/` de votre projet.
2.  Copiez toutes les fonctions définies dans votre notebook (`load_and_explore_data`, `create_train_test_split`, `create_preprocessing_pipeline`, etc.) dans ce fichier.
3.  N'oubliez pas de copier également les **imports** nécessaires en haut du fichier.
4.  À la fin du fichier `src/model_pipeline.py`, ajoutez le bloc suivant pour permettre l'exécution du script :

```python
if __name__ == "__main__":
    # Vous pouvez appeler votre fonction main() ici ou simplement tester une exécution
    print("Exécution du pipeline depuis le script...")
    trained_model = main()
```

5.  **Vérification** :
    *   Ouvrez un terminal dans VS Code.
    *   Placez-vous à la racine du projet.
    *  Activer votre vitual environmenet en faisant `source .venv/bin/activate` ou autre en fonction de votre gestionnaire d'environnement (e.g: conda activate <myenv>).
    *   Exécutez votre script avec la commande :
        ```bash
        python src/model_pipeline.py
        ```
        ou  avec `uv run src/model_pipeline.py`

Si tout fonctionne, vous verrez vos logs s'afficher dans le terminal (et les graphiques s'ouvriront ou seront sauvegardés si configuré). Félicitations, vous avez transformé votre expérimentation en code logiciel "robuste" !
