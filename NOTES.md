# NOTES

- [NOTES](#notes)
  - [Links](#links)
  - [TLDR](#tldr)
  - [Start NestJs NPM Package](#start-nestjs-npm-package)
    - [Clone and change metadata](#clone-and-change-metadata)
    - [Init Git/Remote Repository](#init-gitremote-repository)
    - [Create remote repository NestJsPackageStarter](#create-remote-repository-nestjspackagestarter)
    - [Setting up for development](#setting-up-for-development)
    - [Create the test app / consumer app](#create-the-test-app--consumer-app)
    - [Edit root .gitignore](#edit-root-gitignore)
    - [Update all consumer dependencies to latest and greatest](#update-all-consumer-dependencies-to-latest-and-greatest)
    - [Update starter dependencies to nest 7.0](#update-starter-dependencies-to-nest-70)
    - [Build the package](#build-the-package)
    - [Install the package into the test app](#install-the-package-into-the-test-app)
    - [Use the package in the test app](#use-the-package-in-the-test-app)
    - [Configure/Fix debugger](#configurefix-debugger)
    - [Do some changes in package](#do-some-changes-in-package)
    - [Commit project](#commit-project)
  - [Problems](#problems)
    - [Package.json "dist/test.js"](#packagejson-disttestjs)
    - [Error: Cannot find module 'reflect-metadata'](#error-cannot-find-module-reflect-metadata)

## Links

- [Publishing NestJS Packages with npm](https://dev.to/nestjs/publishing-nestjs-packages-with-npm-21fm)

## TLDR

used node version `node/v12.8.1`

## Start NestJs NPM Package

### Clone and change metadata

```shell
$ git clone https://github.com/nestjsplus/nestjs-package-starter.git
$ mv nestjs-package-starter nestjs-package-starter
# change package name to @koakh/nestjs-auth-package
$ code nestjs-package-starter/package.json
```

```json
{
  "name": "@koakh/nestjs-package-starter",
  "version": "1.0.0",
  "description": "Koakh NestJS Jwt Authentication Package",
  "author": "Mário Monteiro <marioammonteiro@gmail.com>",
  "license": "MIT",
  "readmeFilename": "README.md",
  "main": "dist/index.js",
  "repository": {
    "type": "git",
    "url": "https://github.com/koakh/NestJsPackageStarter"
  },
  "bugs": "https://github.com/koakh/NestJsPackageStarter",
```

### Init Git/Remote Repository

```shell
$ touch README.md
$ git init
$ git add .
$ git commit -am "first commit"
```

### Create remote repository NestJsPackageStarter

```shell
git branch -M main
git remote add origin https://github.com/koakh/NestJsPackageStarter.git
git push -u origin main
```

### Setting up for development

```shell
$ cd nestjs-package-starter/
$ npm i
```

### Create the test app / consumer app

In your second terminal window, make sure you start out in the top level folder you created. Scaffold the small NestJS app we'll be using to exercise our package.

```shell
# open other terminal window
$ nest new nestjs-package-starter-consumer
$ cd nestjs-package-starter-consumer
```

Your folder structure should look similar to this now:

```shell
├── nestjs-package-starter
├── nestjs-package-starter-consumer
├── NOTES.md
└── README.md
```

### Edit root .gitignore

```shell
nestjs-package-starter/node_modules/**
nestjs-package-starter-consumer/node_modules/**
.trash
.bak
```

```shell
$ git add .
$ git commit -am "commit before update starter npm dependencies"
```

### Update all consumer dependencies to latest and greatest

```shell
$ code nestjs-package-starter-consumer/package.json
```

```json
{
  "name": "@koakh/nestjs-package-starter-consumer",
  "version": "1.0.0",
  "description": "Koakh NestJS Jwt Authentication Package",
  "author": "Mário Monteiro <marioammonteiro@gmail.com>",
  "license": "MIT",
  "readmeFilename": "README.md",
  ...
  "dependencies": {
    "@nestjs/common": "^7.4.4",
    "@nestjs/core": "^7.4.4",
    "@nestjs/platform-express": "^7.4.4",
    "reflect-metadata": "^0.1.13",
    "rimraf": "^3.0.2",
    "rxjs": "^6.6.3"
  },
  "devDependencies": {
    "@types/express": "^4.17.8",
    "@types/jest": "^26.0.14",
    "@types/node": "^14.11.2",
    "@types/supertest": "^2.0.10",
    "@nestjs/testing": "^7.4.4",
    "jest": "^26.4.2",
    "nodemon": "^2.0.4",
    "prettier": "^2.1.2",
    "supertest": "^5.0.0",
    "ts-jest": "^26.4.1",
    "ts-node": "^9.0.0",
    "tsconfig-paths": "^3.9.0",
    "tslint": "^6.1.3",
    "typescript": "^4.0.3"
  },
```

resolve dependencies

```shell
$ cd nestjs-package-starter-consumer/
$ rm package-lock.json
$ npm i
```

### Update starter dependencies to nest 7.0

```shell
# open side by side
$ code nestjs-package-starter/package.json
$ code nestjs-package-starter-consumer/package.json
```

paste from consumer app to package

```json
"devDependencies": {
    "@types/express": "^4.17.8",
    "@types/jest": "^26.0.14",
    "@types/node": "^14.11.2",
    "@types/supertest": "^2.0.10",
    "@nestjs/testing": "^7.4.4",
    "jest": "^26.4.2",
    "nodemon": "^2.0.4",
    "prettier": "^2.1.2",
    "supertest": "^5.0.0",
    "ts-jest": "^26.4.1",
    "ts-node": "^9.0.0",
    "tsconfig-paths": "^3.9.0",
    "tslint": "^6.1.3",
    "typescript": "^4.0.3"
```

and remove old duplicated, below are the removed duplicated dependencies

don't forget the update existing

```json
"devDependencies": {
  "@nestjs/common": "^7.4.4",
  "@nestjs/core": "^7.4.4",
  "@nestjs/platform-express": "^7.4.4",
  "tsc-watch": "^4.2.9"
```

now bump `peerDependencies` to 7.0

```json
  "peerDependencies": {
    "@nestjs/common": "^7.0.0"
  },
```

update dependencies

```shell
$ cd nestjs-package-starter
$ rm package-lock.json
$ npm i
```

### Build the package

Take a moment to poke around in the `nestjs-package-starter/nestjs-package-starter` folder

```shell
$ cd nestjs-package-starter
$ npm run build
# commit changes
$ git add . && git commit -am "finished dependencies update"
$ git push
```

### Install the package into the test app

```shell
$ cd nestjs-package-starter-consumer/
# use npm to install the package we just built into our test app.
$ npm install ..
$ npm install ../nestjs-package-starter
```

### Use the package in the test app

The template package exports a single simple test function. Examine `code ../nestjs-package-starter/src/test.ts` to see it:

```typescript
export function getHello(): string {
  return 'Hello from the new package!';
}
```

Now that you've installed the new package in `nestjs-package-starter-consumer`, it's available like any npm package, and you can use it in the normal fashion. Open `nestjs-package-starter-consumer/src/app.controller.ts` and import the function; make sure the file looks like this:

```shell
import { Controller, Get } from '@nestjs/common';
import { getHello } from '@koakh/nestjs-package-starter';

@Controller()
export class AppController {
  @Get()
  getHello(): string {
    return getHello();
  }
}
```

In terminal window 2, start `nestjs-package-starter` (using start:dev is recommended here so we can make iterative changes):

```shell
$ cd nestjs-package-starter-consumer
$ npm run start:dev
# done we have access to package
$ curl localhost:3000
Hello from the new package!
```

### Configure/Fix debugger

we must fix `nestjs-package-starter-consumer` `npm run start:debug`, else it won't start as expected, change

`nestjs-package-starter-consumer/nodemon-debug.json`

remove `-brk` from `--inspect-brk`

```json
{
  "exec": "node --inspect -r ts-node/register -r tsconfig-paths/register src/main.ts"
}
```

add `../nestjs-package-starter/dist` to `watch`, this way we have hot reload working with `start:dev` and `start:debug` in consumer app

`nestjs-package-starter-consumer/nodemon.json`
`nestjs-package-starter-consumer/nodemon-debug.json`

```json
{
  "watch": ["src", "../nestjs-package-starter/dist"],
  ...
}
```

### Do some changes in package

```shell
# in window 1 : nestjs-package-starter-consumer
$ npm run start:debug
# in window 2 : nestjs-package-starter
$ npm run start:dev
```

Make a simple change to the `nestjs-package-starter/src/test.ts` `getHello()` function exported by the package, change it to return 'Buon Giorno!'

> Note: use `npm run start:debug` and add a `breakpoint/debugger;`

```typescript
export function getHello(): string {
  debugger;
  return 'Buon Giorno!';
}
```

Notice that in terminal window 2, since we're linked to the local package, the dev server will automatically restart as the package is rebuilt :)

```shell
$ curl localhost:3000
Buon Giorno!
```

now we have our development environment ready to roll

### Commit project

commit id [8b0737b](https://github.com/koakh/NestJsPackageStarter/commit/8b0737b24454bad1641c0182190824f1b2cc54aa)

```shell
$ git add .
[main 8b0737b] now we have our development environment ready to roll
```

## Problems

### Package.json "dist/test.js"

problems detected in authentication package

```shell
$ npm run start
TSError: ⨯ Unable to compile TypeScript:
src/app.module.ts:4:28 - error TS2307: Cannot find module '@koakh/nestjs-package-jwt-authentication' or its corresponding type declarations.

4 import { AuthModule } from '@koakh/nestjs-package-jwt-authentication';
```

fuck the problem is this `"main": "dist/test.js"`, change to "main": `"dist/index.js"`

```json
{
  "name": "@koakh/nestjs-package-jwt-authentication",
  "version": "1.0.0",
  "description": "Koakh NestJS Jwt Authentication Package",
  "author": "Mário Monteiro <marioammonteiro@gmail.com>",
  "license": "MIT",
  "readmeFilename": "README.md",
  "main": "dist/test.js",
```

and re-install `npm i ../nestjs-package-jwt-authentication`

### Error: Cannot find module 'reflect-metadata'

```shell
 cd nestjs-package-jwt-authentication-consumer
$ npm run start
Error: Cannot find module 'reflect-metadata'
required in `nestjs-package-jwt-authentication`
Require stack:
- /media/mario/Storage/Documents/Development/Node/@NestJsPackages/TypescriptNestJsPackageJwtAuthentication/nestjs-package-jwt-authentication/node_modules/@nestjs/common/index.js
```

- [Cannot find module 'reflect-metadata](https://github.com/nestjs/nest/issues/1211)

It's a `peerDependency`, You need to install it alongside rxjs aswell.

```shell
# install in package
$ cd nestjs-package-jwt-authentication
$ npm install --save reflect-metadata rxjs
```

done