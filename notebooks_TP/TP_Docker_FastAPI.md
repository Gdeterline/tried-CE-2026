# TP : Dockeriser votre API FastAPI

Dans ce TP, nous allons apprendre à "conteneuriser" notre API. Cela permet de s'assurer qu'elle fonctionnera de la même manière sur n'importe quelle machine (votre ordinateur, un serveur, le cloud, etc.) sans avoir à se soucier des problèmes d'installation de dépendances ou de versions de Python.

## Objectifs

1.  Comprendre la structure d'un `Dockerfile`.
2.  Construire une **Image** Docker.
3.  Lancer un **Conteneur** Docker à partir de cette image.

---

## Étape 1 : Créer le Dockerfile

Le `Dockerfile` est la "recette de cuisine" qui permet à Docker de construire votre image.

1.  À la racine de votre projet (là où se trouvent `main.py`, `requirements.txt`, etc.), créez un nouveau fichier nommé `Dockerfile` (sans aucune extension).
2.  Copiez-y le contenu suivant :

```dockerfile
# 1. Image de base
# On part d'une version légère (slim) de Python 3.13 officielle.
FROM python:3.13-slim

# 2. Dossier de travail
# On définit le dossier /app dans le conteneur comme dossier courant.
WORKDIR /app

# 3. Copie des dépendances
# On copie le fichier requirements.txt de votre machine vers le conteneur.
COPY requirements.txt .

# 4. Installation des dépendances
# On installe les librairies Python listées dans le fichier.
# --no-cache-dir permet de réduire la taille de l'image.
RUN pip install --no-cache-dir -r requirements.txt

# 5. Copie du code source
# On copie le dossier 'api' de votre machine dans le dossier /app/api du conteneur.
COPY api/ ./api/

# 6. Documentation du port
# On indique que le conteneur écoutera sur le port 8000.
EXPOSE 8000

# 7. Commande de démarrage
# C'est la commande qui sera exécutée quand on lancera le conteneur.
# On lance uvicorn pour servir l'API.
CMD ["uvicorn", "api.iris_api:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Points importants :
*   `FROM` : Toujours la première ligne. Définit l'OS et l'environnement de base.
*   `COPY` : Attention au contexte ! Le premier chemin est sur votre machine (relatif au Dockerfile), le second est dans le conteneur.
*   `--host 0.0.0.0` : Crucial dans un conteneur Docker. Si vous laissez `127.0.0.1` (le défaut), l'API ne sera accessible **que** depuis l'intérieur du conteneur (donc inaccessible pour vous).

---

## Étape 2 : Construire l'image (Build)

Maintenant que nous avons la recette, préparons le plat (l'image).

Ouvrez votre terminal à la racine du projet et exécutez :

```bash
docker build -t iris-fastapi .
```

**Explication de la commande :**
*   `docker build` : Demande à Docker de construire une image.
*   `-t iris-fastapi` : L'option "tag". On donne le nom `iris-fastapi` à notre image pour pouvoir la retrouver facilement.
*   `.` (le point à la fin) : **Très important**. Il indique à Docker que le "contexte" (les fichiers à utiliser) se trouve dans le dossier courant. Docker va chercher le `Dockerfile` ici.

*Si c'est la première fois, cela peut prendre quelques minutes le temps de télécharger l'image Python.*

---

## Étape 3 : Lancer le conteneur (Run)

Notre image est prête. Lançons-la !

```bash
docker run -p 8000:8000 iris-fastapi
```

**Explication de la commande :**
*   `docker run` : Lance un conteneur.
*   `-p 8000:8000` : **Mappage de ports**.
    *   Le premier `8000` est le port de **votre machine** (host).
    *   Le second `8000` est le port du **conteneur** (celui défini dans le EXPOSE et le CMD).
    *   Cela signifie : "Tout ce qui arrive sur mon port 8000, envoie-le au port 8000 du conteneur".
*   `iris-fastapi` : Le nom de l'image à utiliser.

---

## Étape 4 : Tester

Si tout s'est bien passé, vous devriez voir des logs indiquant qu'Uvicorn a démarré.

Ouvrez votre navigateur et allez sur :
[http://localhost:8000/docs](http://localhost:8000/docs)

Vous devriez voir votre API Swagger UI, exactement comme quand vous la lanciez localement, mais cette fois, elle tourne isolée dans un conteneur Docker !

---

## Commandes utiles pour le nettoyage

Une fois le TP terminé :

1.  Arrêtez le conteneur avec `CTRL+C` dans le terminal.
2.  Pour voir les conteneurs qui tournent encore en arrière-plan :
    ```bash
    docker ps
    ```
3.  Pour supprimer une image (si vous voulez faire de la place) :
    ```bash
    docker rmi iris-fastapi
    ```
