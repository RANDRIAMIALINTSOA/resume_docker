
# Resume Docker :

## 1. Introduction à Docker

### Qu'est-ce que Docker ?

Docker est une plateforme permettant de ** créer**, *déployer et * exécuter des applications dans des *conteneurs.

Un conteneur est une version légère d'une machine virtuelle, qui regroupe une application avec toutes ses dépendances, ce qui garantit que le code s'exécutera de la même manière partout.



## 2. Avantages de Docker

- Isolation des applications
- Légèreté des conteneurs
- Déploiement rapide
- Portabilité entre environnements
- Meilleure gestion des dépendances

---

## 3. Commandes Docker de base

### Images et Conteneurs

```bash
# Télécharger une image depuis Docker Hub
docker pull <nom_de_l_image>

# Lister les images disponibles
docker images

# Supprimer une image
docker rmi <image_id>
```

### Gestion des conteneurs

```bash
# Exécuter un conteneur
docker run <options> <nom_image>

# Exemple : exécuter nginx sur le port 8080
docker run -d -p 8080:80 nginx

# Lister les conteneurs actifs
docker ps

# Lister tous les conteneurs (même arrêtés)
docker ps -a

# Stopper un conteneur
docker stop <container_id>

# Supprimer un conteneur
docker rm <container_id>
```

### Volumes (données persistantes)

```bash
# Créer un volume
docker volume create mon_volume

# Lister les volumes
docker volume ls

# Supprimer un volume
docker volume rm mon_volume
```

### Dockerfile (automatiser la création d'image)

Un fichier `Dockerfile` permet de définir une image personnalisée :

```Dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```

Construire l'image :

```bash
docker build -t mon_app .
```

---

## 4. Docker Compose

Le fichier `docker-compose.yml` permet de gérer plusieurs services ensemble (ex. : application + base de données).

Exemple :

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: exemple
```

Lancer les services :

```bash
docker-compose up -d
```

---

## 5. Réseaux, Swarm et Résilience

### Euphemère

- Réseaux de type *overlay* (abstraction de réseau)
- Montée en charge automatique (*auto-scaling*) et *auto-réparation* (*self-healing*)
- Exemple : redémarrage automatique si un conteneur échoue
- *Load balancing* :
  - Ajout d’un nouveau service → accès automatique via l’image mise à jour (ex: image v2)

---

### Réplication des conteneurs

```bash
nginx.1 et nginx.2 sont des *réplicas* du même conteneur.
```

---

### Problèmes de résilience

**SPOF** : Single Point of Failure → un point unique de défaillance peut entraîner l'arrêt de tout le système.

Solution : Répliquer les composants critiques dans un cluster.

---

### Architecture du Cluster

- **manager / maître / control plane** : Responsable de la gestion du cluster
- **worker / esclave / dataplane** : Exécute les tâches
- **ZDT** : Zone de Tolérance de Défaillance (Zero Downtime Tolerance)

---

### Calcul du Quorum

**Définitions** :
- n = nombre de managers
- quorum = `floor(n / 2) + 1` (majorité absolue)
- tolérance = `n - quorum`

**Exemple** :
```text
Si n = 5 :
quorum = floor(5 / 2) + 1 = 3
tolérance = 5 - 3 = 2
→ Le système peut tolérer la perte de 2 managers
```

---

### Bonnes pratiques

- Le nombre de managers doit être impair pour éviter les blocages lors des votes.
- Utilisation d’un algorithme d’élection pour désigner un leader parmi les managers.

---

## 6. Résumé Visuel

Cluster :
- Manager (control plane)
- Worker (dataplane)
- ZDT (zone de tolérance)

Exemple :
```text
n = 5
quorum = 3
tolérance = 2
```

---

## 7. Vérification et Nettoyage

```
bash
# Vérifier l'espace utilisé
docker system df

# Nettoyer les ressources inutilisées
docker system prune

```

---

## 8. Ressources Utiles

- [Docker Documentation](https://docs.docker.com/)
- [DockerHub](https://hub.docker.com/)
- [Play with Docker](https://labs.play-with-docker.com/)

---

*Créé par Mialy RANDRIAMIALINTSOA – Leçon Docker complète simplifiée pour débutants.*
