# BACKEND EN NODEJS AVEC TYPESCRIPT

1 - Creation du dossier de travail

```bash
$ mkdir backend && cd backend
```

2 - Construire l'environement node

```bash
$ npm init -y
Wrote to /home/nresh/workspace/sunem/backend/package.json:

{
  "name": "backend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

3 - Installer Typescript

```bash
$ npm install --save-dev typescript ts-node-dev @types/node

npm WARN deprecated inflight@1.0.6: This module is not supported, and leaks memory. Do not use it. Check out lru-cache if you want a good and tested way to coalesce async requests by a key value, which is much more comprehensive and powerful.
npm WARN deprecated rimraf@2.7.1: Rimraf versions prior to v4 are no longer supported
npm WARN deprecated glob@7.2.3: Glob versions prior to v9 are no longer supported

added 66 packages, and audited 67 packages in 16s

9 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

4 - Initialiser TypeScript

```bash
$ npx tsc --init


Created a new tsconfig.json                                                                         
                                                                                                 TS 
You can learn more at https://aka.ms/tsconfig

```

5 - Modifier le tsconfig.json 

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "rootDir": "src",
    "outDir": "dist",
    "esModuleInterop": true,
    "strict": true
  }
}
```

6 - Creation de la structure du projet

```pgsql
backend/
│
├── node_modules/
├── src/
│   │
│   ├── models/
│   │   │
│   │   └──root.models.ts
├── .env
├── .env.example
├── .gitignore
├── package-lock.json
├── package.json
├── README.md
├── tsconfig.json
├──
├──
├──
├──
├──
└──
```

7 - Ajouter des scripts dans package.json

```json
"scripts": {
  "dev": "ts-node-dev --respawn --transpile-only src/server.ts",
  "build": "tsc",
  "start": "node dist/server.js"
}
```

8 - installation des module

```bash
# installer mongoose pour la base de donnée
$ npm install mongoose                                     

added 18 packages, and audited 85 packages in 15s

10 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities


# installer dotenv pour le controller des variable d'environement
$ npm install dotenv  

added 1 package, and audited 86 packages in 3s

11 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

```





``````