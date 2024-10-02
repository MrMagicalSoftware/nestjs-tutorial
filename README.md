
Web site to draw online :
https://excalidraw.com/

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

In NestJS, a popular framework for building scalable and maintainable server-side applications with Node.js, modules play a central role in organizing and structuring your application. They help in managing the application's architecture by grouping related components, such as controllers, providers (services), and other modules, into cohesive units. 
This modular approach enhances code maintainability, reusability, and scalability.


What Are Modules in NestJS?

A module in NestJS is a class annotated with the @Module() decorator. It serves as a container that encapsulates a set of related components, including:

    Controllers: Handle incoming requests and return responses.
    Providers: Services or other injectable classes that contain business logic.
    Imports: Other modules whose exported providers are required in the current module.
    Exports: Providers that should be available to other modules that import this module.

Modules help in organizing your application into distinct features or domains, making the codebase easier to manage, especially as the application grows in complexity.


**Defining a Module**

Here's a basic example of how to define a module in NestJS:

```
import { Module } from '@nestjs/common';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';
import { DatabaseModule } from '../database/database.module';

@Module({
  imports: [DatabaseModule], // Import other modules if needed
  controllers: [UsersController], // Controllers related to this module
  providers: [UsersService], // Providers (services) for this module
  exports: [UsersService], // Export providers to make them available in other modules
})
export class UsersModule {}
```




For generating a module u can use : 

```
nest generate module NameOfModules
```
For creare a new controller to a module u can use : 
```
nest g controller NameOfModules
```

For a service :
```
nest g service ninjas
```

For creating controller and provider in one command :

```
nest g resource NameofResourse
```






# Controllers



























