# nestjs-tutorial

# Setup
# Modules
# Controllers
# Providers
# Exception Handling
# Pipes
# Guards




Nestjs uses the ExpressJs like the base ;
it's build on top of nestjs.

Find more on : https://docs.nestjs.com/recipes/documentation

# how to install nestjs

```
npm install -g @nestjs/cli@latest

```

Create a new Project :

```
nest new ninja-api
```

Open the project with Visual Studio Code and the see the file named package.json:
In this file u can see a list of the main commands used in nestjs like :
nest start , or **nest start --watch**

The entry point of the app is main.ts

```
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```
______________________________________________

Start the app with the relative command :


I used : http://localhost:3000 with Rested client :
(Install rested client ) :
Available for https://addons.mozilla.org/it/firefox/addon/rested/

Make the Get Request with Rested : http://localhost:3000

 HTTP GET  -> controller -> service  and after service -> controller -> GET


______________________________________________


# Modules :








































