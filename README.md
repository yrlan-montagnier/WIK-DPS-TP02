# WIK-DPS-TP02

## Créer une image Docker avec un seul stage qui permet d’exécuter votre API développée précédemment (WIK-DPS-TP01)
[dockerfile](dockerfile)
```
FROM node as builder

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

RUN npx tsc

CMD node build/index.js

USER node
```

## L'image doit être la plus optimisée possible concernant l'ordre des layers afin de limiter le temps de build lors des modifications sur le code
Le build de l'image docker à partir du dockerfile se fait via la commande `docker build -t api_typescript_docker .`, il prend environ ~10s à se faire.
    ![Build_Image](/img/build_image.png)

## Scanner votre image avec docker scan, trivy ou clair pour obtenir la liste des vulnérabilités détectées
Pour scanner mon image, j'ai utilisé trivy avec la commande trivy image `api_typescript_docker`.
    ![Trivy](./img/Trivy.png)

## L'image doit utiliser un utilisateur spécifique pour l'exécution de votre serveur web
Dans le dockerfile, on a rajouté `USER node`, qui est intégré à node et permet de lancer le conteneur avec cet utilisateur.

Cette commande permet de retourner l'utilisateur sur lequel sont lancés les différents conteneurs en cours :
```
yrlan@MSI-9SEXR:~$ docker inspect $(docker ps -q) --format '{{.Config.User}} {{.Name}}'
node /API_TypeScript_Yrlan
```

## Créer une seconde image Docker pour votre API avec les mêmes contraintes en termes d'optimisations mais avec plusieurs stages : un pour l'étape de build et une autre pour l’exécution (qui ne contient pas les sources)

## Pour lancer le projet :
1. Cloner le repo avec `git clone https://github.com/yrlan-montagnier/WIK-DPS-TP02.git`
2. Ouvrir le dossier dans vscode ou dans un terminal et éxecutez ces commandes :
    - `docker build -t api_typescript_docker .`
    - `docker run -d -p 8080:8080 --name API_TypeScript_Yrlan api_typescript_docker`