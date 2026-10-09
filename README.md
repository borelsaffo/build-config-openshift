# build-config-openshift
Les stratégies de build dans OpenShift

OpenShift propose plusieurs stratégies de build pour transformer le code source d'une application en une image exécutable. Dans notre configuration, nous utilisons deux stratégies : Source-to-Image (S2I) et Docker.

1. Stratégie Source-to-Image (S2I)

La stratégie Source permet de construire une image à partir du code source d'une application, sans avoir besoin d'écrire un Dockerfile dans le cas classique.

Configuration YAML
strategy:
  type: Source
  sourceStrategy:
    from:
      kind: "ImageStreamTag"
      name: "python:3.6"

Fonctionnement
OpenShift récupère le code source depuis le dépôt Git.
Il utilise l'image de base python:3.6 pour fournir l'environnement Python.
Le processus S2I installe les dépendances nécessaires et prépare l'application.
Une image contenant l'application prête à être exécutée est générée.
Avantages
Simplifie le processus de construction.
Évite d'avoir à écrire un Dockerfile dans le cas classique.
Automatise la préparation de l'environnement d'exécution.
Convient aux applications utilisant des langages et environnements pris en charge par S2I.
2. Stratégie Docker

La stratégie Docker permet de construire une image en suivant les instructions définies dans un Dockerfile.

Configuration YAML
strategy:
  type: Docker
  dockerStrategy:
    from:
      kind: "DockerImage"
      name: "ubuntu:16.04"

Fonctionnement
OpenShift récupère le code source depuis le dépôt Git.
Il recherche le Dockerfile dans le contexte de construction.
Il exécute les instructions du Dockerfile pour construire l'image.
L'image obtenue est publiée dans la destination configurée dans le BuildConfig.
Avantages
Offre un contrôle précis sur la construction de l'image.
Permet de personnaliser l'installation des dépendances.
Permet de configurer l'environnement et les commandes de démarrage.
Convient aux applications nécessitant des étapes de construction spécifiques.
3. Comparaison entre S2I et Docker
Critère	S2I (Source)	Docker (Docker)
Dockerfile	Généralement non nécessaire	Nécessaire
Construction	Automatisée par le processus S2I	Définie par les instructions du Dockerfile
Personnalisation	Dépend de l'image S2I et des scripts utilisés	Contrôle détaillé via le Dockerfile
Simplicité	Plus simple pour les environnements pris en charge	Nécessite de gérer le Dockerfile
Utilisation	Applications compatibles avec S2I	Applications nécessitant une construction personnalisée
4. Conclusion

La stratégie S2I est recommandée lorsque l'on souhaite construire rapidement une application à partir de son code source, en s'appuyant sur une image adaptée au langage utilisé.

La stratégie Docker est préférable lorsque l'on souhaite maîtriser précisément les étapes de construction de l'image à l'aide d'un Dockerfile.

Remarque : les images python:3.6 et ubuntu:16.04 utilisées dans les exemples sont anciennes. Pour un environnement réel, il est préférable de choisir des images encore maintenues et compatibles avec la version d'OpenShift utilisée.
