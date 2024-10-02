
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

**Breakdown of the @Module() Decorator**

imports: An array of modules required by this module. Importing a module allows you to use its exported providers within the current module.
controllers: An array of controllers that are part of this module. Controllers handle incoming HTTP requests and return responses.
providers: An array of providers (services, repositories, factories, etc.) that are instantiated by NestJS's dependency injection system and can be injected into other components.
exports: An array of providers that should be available to other modules that import this module. This is essential for sharing services across different parts of the application.


**The Root Module**

Every NestJS application has at least one module, the root module, which is typically named AppModule. This module serves as the entry point of the application and imports other feature modules.

```
import { Module } from '@nestjs/common';
import { UsersModule } from './users/users.module';
import { AuthModule } from './auth/auth.module';

@Module({
  imports: [UsersModule, AuthModule],
})
export class AppModule {}
```

**Feature Modules**

As your application grows, you can create feature modules to encapsulate related functionality. 
For example, a UsersModule might handle all user-related operations, while an AuthModule manages authentication.


**Example: Creating a Feature Module**

Generate Module, Controller, and Service:
NestJS provides a CLI to generate modules, controllers, and services easily:

```
nest generate module users
nest generate controller users
nest generate service users
```

**Implement the Module:**

```
// users.module.ts
import { Module } from '@nestjs/common';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';

@Module({
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService],
})
export class UsersModule {}
```

**Use the Module in Root Module:**

```
// app.module.ts
import { Module } from '@nestjs/common';
import { UsersModule } from './users/users.module';

@Module({
  imports: [UsersModule],
})
export class AppModule {}
```



## Dynamic Modules

NestJS also supports dynamic modules, which are modules that can be configured dynamically during the application bootstrap process. This is useful for scenarios like configuring database connections or third-party integrations based on environment variables.
Example: Creating a Dynamic Module

```
import { Module, DynamicModule } from '@nestjs/common';

@Module({})
export class ConfigModule {
  static forRoot(options: ConfigOptions): DynamicModule {
    return {
      module: ConfigModule,
      providers: [
        {
          provide: 'CONFIG_OPTIONS',
          useValue: options,
        },
        ConfigService,
      ],
      exports: ['CONFIG_OPTIONS', ConfigService],
    };
  }
}
```


You can then import this dynamic module in your root module with specific configurations:

```
import { Module } from '@nestjs/common';
import { ConfigModule } from './config/config.module';

@Module({
  imports: [
    ConfigModule.forRoot({
      envFilePath: '.env',
    }),
  ],
})
export class AppModule {}
```



## Global Modules

Sometimes, you might have modules that should be available throughout the entire application without needing to import them in every other module. These are known as global modules.
Example: Creating a Global Module

```
import { Module, Global } from '@nestjs/common';
import { LoggerService } from './logger.service';

@Global()
@Module({
  providers: [LoggerService],
  exports: [LoggerService],
})
export class LoggerModule {}
```
By annotating the module with @Global(), NestJS makes the LoggerService available globally, eliminating the need to import LoggerModule in every module that requires logging functionality.


## Best Practices for Using Modules

    Feature-Based Organization: Group related controllers, services, and other providers into feature modules to encapsulate functionality.

    Keep Modules Focused: Each module should have a clear responsibility. Avoid creating modules that handle unrelated concerns.

    Use Shared Modules for Common Providers: If multiple modules need the same provider (e.g., utility services), create a shared module that exports these providers.

    Leverage Dynamic and Global Modules When Appropriate: Use dynamic modules for configurable features and global modules for cross-cutting concerns that should be accessible application-wide.

    Avoid Circular Dependencies: Ensure that modules do not depend on each other in a way that creates a cycle, as this can lead to runtime errors and complicate the dependency injection graph.



_______________________

**useful commands**

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









__________________




# Controllers



























