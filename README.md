
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


In **NestJS**, **controllers** are responsible for handling incoming HTTP requests and returning responses to the client. They act as the "entry points" for requests, coordinating the communication between the client (such as a web or mobile app) and the backend services or databases. Controllers define routes that can be mapped to specific HTTP methods, such as `GET`, `POST`, `PUT`, and `DELETE`.

### Key Responsibilities of Controllers
- **Route Handling**: Controllers define the routes that respond to client requests. Each route is associated with an HTTP method.
- **Input Validation**: Controllers can use decorators and DTOs (Data Transfer Objects) to validate incoming data.
- **Response Management**: Controllers send appropriate responses to the client, often by returning the data directly or delegating to services.
- **Delegation**: Controllers delegate the business logic to **services** or **providers**, keeping the controller logic lean and focused on routing.

## Defining a Controller

A controller is defined using the `@Controller()` decorator. This decorator indicates that the class is a controller and can handle HTTP requests.

Here’s an example of a basic controller in NestJS:

```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('users') // This controller handles routes starting with '/users'
export class UsersController {
  @Get() // This method handles GET requests to '/users'
  findAll(): string {
    return 'This action returns all users';
  }
}
```

### Understanding the Example

- **`@Controller('users')`**: This decorator specifies that all routes within the `UsersController` will start with `/users`. For example, the `findAll()` method will handle `GET /users` requests.
  
- **`@Get()`**: This decorator maps the method to handle `GET` requests. Other decorators such as `@Post()`, `@Put()`, `@Delete()` are used for other HTTP methods.

### Handling Different HTTP Methods

NestJS supports various HTTP methods, and you can use different decorators to map these methods to specific routes in your controllers.

Example:

```typescript
import { Controller, Get, Post, Put, Delete, Param } from '@nestjs/common';

@Controller('users')
export class UsersController {
  @Get()
  findAll(): string {
    return 'This action returns all users';
  }

  @Get(':id')
  findOne(@Param('id') id: string): string {
    return `This action returns the user with ID: ${id}`;
  }

  @Post()
  create(): string {
    return 'This action creates a new user';
  }

  @Put(':id')
  update(@Param('id') id: string): string {
    return `This action updates the user with ID: ${id}`;
  }

  @Delete(':id')
  remove(@Param('id') id: string): string {
    return `This action removes the user with ID: ${id}`;
  }
}
```

### Route Parameters and Query Parameters

- **Route Parameters**: Use the `@Param()` decorator to extract route parameters from the request URL.
  
  Example:
  
  ```typescript
  @Get(':id')
  findOne(@Param('id') id: string): string {
    return `User ID: ${id}`;
  }
  ```

  If the request is `GET /users/5`, the controller method extracts the value `5` as the `id`.

- **Query Parameters**: Use the `@Query()` decorator to extract query parameters from the URL.

  Example:
  
  ```typescript
  @Get()
  find(@Query('role') role: string): string {
    return `User role: ${role}`;
  }
  ```

  For the request `GET /users?role=admin`, the method extracts `role=admin`.

### Passing Data with POST and PUT Requests

In NestJS, you can handle data in `POST` and `PUT` requests by using the `@Body()` decorator, which extracts the payload from the request body.

Example:

```typescript
import { Controller, Post, Body } from '@nestjs/common';

@Controller('users')
export class UsersController {
  @Post()
  create(@Body() createUserDto: CreateUserDto): string {
    return `User created: ${createUserDto.name}`;
  }
}
```

In this example:
- **`@Body()`**: Extracts the body of the incoming request, typically JSON data.
- **`CreateUserDto`**: A DTO (Data Transfer Object) that represents the structure of the data the controller expects.

### Using Services with Controllers

It’s a best practice to delegate business logic to **services**, keeping controllers lightweight and focused on routing.

Example:

```typescript
import { Controller, Get } from '@nestjs/common';
import { UsersService } from './users.service';

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll(): string {
    return this.usersService.findAll(); // Delegating logic to the UsersService
  }
}
```

In this case, the `UsersController` uses dependency injection to get an instance of `UsersService`, where the actual logic for retrieving users is handled.

### Status Codes and Responses

You can control the response status codes and headers by using decorators like `@HttpCode()`, `@Header()`, and `@Res()` to send custom responses.

Example:

```typescript
import { Controller, Post, HttpCode, Header } from '@nestjs/common';

@Controller('users')
export class UsersController {
  @Post()
  @HttpCode(201) // Sets the HTTP status code to 201 (Created)
  @Header('Cache-Control', 'none') // Sets a custom header
  create(): string {
    return 'User created';
  }
}
```

### Interacting with the Response Object

Sometimes, you might need direct access to the raw response object. In NestJS, you can inject it using `@Res()`.

```typescript
import { Controller, Get, Res } from '@nestjs/common';
import { Response } from 'express';

@Controller('users')
export class UsersController {
  @Get('file')
  sendFile(@Res() res: Response): void {
    res.sendFile('/path/to/file');
  }
}
```

In this example, you use the `@Res()` decorator to access the underlying Express response object, allowing more control over the response behavior.

## Summary

- **Controllers** in NestJS handle routing and input/output processing.
- Controllers should be lean and delegate business logic to **services**.
- They define routes using decorators like `@Get()`, `@Post()`, `@Put()`, etc., and can extract data from the request (route parameters, query parameters, request body) using decorators like `@Param()`, `@Query()`, and `@Body()`.
- You can manage HTTP responses directly using `@HttpCode()`, `@Header()`, and `@Res()`.

By keeping your controllers focused on handling HTTP requests and using services for business logic, you ensure that your code remains clean, maintainable, and scalable.
























