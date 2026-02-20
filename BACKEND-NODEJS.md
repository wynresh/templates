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
│   ├── controllers
│   │   │
│   │   ├── sign.controllers.ts
│   │   └── user.controllers.ts
│   │
│   ├── models/
│   │   │
│   │   ├── user.models.ts
│   │   └── root.models.ts
│   │
│   ├── services
│   │   │
│   │
│   ├── utils
│   │
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


# installer zod pour appliquer une validation sur les champs de donné
$ npm install zod   

added 1 package, and audited 87 packages in 3s

12 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities


# installer mongoose-paginate-v2 pour la pagination
$ npm install mongoose-paginate-v2

added 3 packages, and audited 90 packages in 3s

12 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities


# installer bcrypt pour hasher le mot de pass
$ npm install bcrypt
npm install --save-dev @types/bcrypt

added 3 packages, and audited 94 packages in 6s

12 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

added 1 package, and audited 95 packages in 3s

12 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities


# installer libphonenumber-js pour validé le numero et le formater
$ npm install libphonenumber-js   

added 1 package, and audited 91 packages in 7s

12 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities


# installer express pour le server
$ npm install express      
npm install --save-dev @types/express        

added 59 packages, and audited 154 packages in 5s

33 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities


# installer nodemailer pour les service mail
$ npm install nodemailer
npm install --save-dev @types/nodemailer


# installer jsonwebtoken pour la gestion des token
$ npm install jsonwebtoken
npm install --save-dev @types/jsonwebtoken              

added 15 packages in 3s

1 package is looking for funding
  run `npm fund` for details


# installer qrcode pour generer une image qrcode pour google authenticator
$ npm install speakeasy qrcode
npm install --save-dev @types/qrcode


added 31 packages, and audited 212 packages in 7s

36 packages are looking for funding
  run `npm fund` for details

5 vulnerabilities (1 low, 4 high)

To address issues that do not require attention, run:
  npm audit fix

Some issues need review, and may require choosing
a different dependency.

Run `npm audit` for details.


# installer speakeasy pour generer le code secret pour google authenticator
$ npm install speakeasy speakeasy
npm install --save-dev @types/speakeasy

up to date, audited 212 packages in 1s

36 packages are looking for funding
  run `npm fund` for details

5 vulnerabilities (1 low, 4 high)

To address issues that do not require attention, run:
  npm audit fix

Some issues need review, and may require choosing
a different dependency.

Run `npm audit` for details.
```

9- generer un secret code

```bash
$ openssl rand -hex 64
```

10 - contenus des fichiers et dossiers

10.1 - Models: dir


```ts
// ======================
// file: root.models.ts
// Root Models: 
// 
// configuration commune à tous les modèles Mongoose pour utiliser id au lieu de _id,
// et pour configurer la sérialisation JSON et Object.
// ======================

import { Schema } from "mongoose";

const root = (schema: Schema) => {
    // Virtual id
    schema.virtual("id").get(function (this: Document & { _id: any }) {
        return this._id.toHexString();
    });

    // toJSON config
    schema.set("toJSON", {
        virtuals: true,
        versionKey: false,
        transform: (_, ret) => {
            delete ret._id;  // masquer _id
        },
    });

    // toObject config (optional)
    schema.set("toObject", {
        virtuals: true,
        versionKey: false,
    });
};

export default root;



```



``````