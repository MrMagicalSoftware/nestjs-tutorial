
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



____________________________________________________________


# Providers 


## Key concepts

- **Providers** are classes or values in NestJS that handle the core logic of the application, such as services, repositories, or utilities.
- Providers are decorated with `@Injectable()` and can be injected into other classes (like controllers or other services) using **Dependency Injection**.
- Providers are registered in the `providers` array of a module and can have different scopes (singleton, request-scoped, or transient).
- Custom providers allow you to inject values, configurations, or dynamic services.
- **Global providers** can be made available application-wide through global modules.

By using providers, NestJS promotes the separation of concerns and modularity, helping to build maintainable and scalable applications.


In **NestJS**, **providers** are classes or objects that can be **injected** into other classes (such as controllers or other providers) via **Dependency Injection (DI)**. Providers are responsible for encapsulating **business logic** and **reusable functionality** within your application. They are typically used to manage the core operations, services, or utilities that your application needs.

### Key Responsibilities of Providers:
- Encapsulate business logic, utilities, and services.
- Be reusable components that can be injected into other parts of the application (e.g., controllers, other services).
- Serve as a layer of abstraction between your application's controllers and the core business logic.

In NestJS, services are the most common type of provider, but other types (like repositories, factories, or even third-party libraries) can also be registered as providers.

## Defining a Provider

A provider in NestJS is usually a class annotated with the `@Injectable()` decorator, which marks it as a provider that can be injected into other classes.

Example:

```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class UsersService {
  private users = ['John', 'Jane'];

  findAll(): string[] {
    return this.users;
  }

  create(user: string) {
    this.users.push(user);
  }
}
```

### Explanation:
- **`@Injectable()`**: This decorator marks the `UsersService` class as a provider, making it eligible for dependency injection.
- **`findAll()`** and **`create()`**: These methods encapsulate the logic to interact with the `users` array (which simulates a simple in-memory data store in this example).

## Using Providers in Controllers

Once you define a provider, you can **inject** it into a controller (or another provider) using **constructor-based injection**.

Example:

```typescript
import { Controller, Get, Post, Body } from '@nestjs/common';
import { UsersService } from './users.service';

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  findAll(): string[] {
    return this.usersService.findAll(); // Delegates the logic to UsersService
  }

  @Post()
  create(@Body('name') name: string) {
    this.usersService.create(name); // Uses UsersService to create a new user
  }
}
```

### Explanation:
- **Constructor Injection**: The `UsersService` is injected into the `UsersController` via the constructor (`private readonly usersService: UsersService`).
- **Delegating Logic**: The controller delegates the actual business logic to the `UsersService`, which follows the principle of separation of concerns.

## Dependency Injection

NestJS uses a **dependency injection** (DI) system to handle how providers are shared and reused throughout the application. When a provider is requested (e.g., in a controller), NestJS **resolves** its dependencies and provides the instance, ensuring the right provider is injected where needed.

### Injecting Providers into Other Providers

Providers can also be injected into other providers, which is useful for building complex services that depend on each other.

Example:

```typescript
import { Injectable } from '@nestjs/common';
import { UsersService } from './users.service';

@Injectable()
export class AuthService {
  constructor(private readonly usersService: UsersService) {}

  validateUser(userName: string): boolean {
    const users = this.usersService.findAll();
    return users.includes(userName);
  }
}
```

In this example:
- The `AuthService` depends on `UsersService`, and NestJS resolves the dependency when it instantiates `AuthService`.

## Registering Providers in a Module

Providers must be registered in a **module** to be available for injection. You register providers using the `providers` array in the `@Module()` decorator.

Example:

```typescript
import { Module } from '@nestjs/common';
import { UsersService } from './users.service';
import { UsersController } from './users.controller';

@Module({
  controllers: [UsersController],
  providers: [UsersService], // Registering UsersService as a provider
})
export class UsersModule {}
```

Here, the `UsersService` is registered as a provider in the `UsersModule`, making it available for injection in the `UsersController` (and any other classes that are part of the module).

## Scope of Providers

By default, providers in NestJS are **singleton**. This means that the same instance of a provider is shared across the entire application. However, you can also define **scoped providers**, which are created for every request or explicitly instantiated when needed.

### Types of Provider Scopes:
1. **Singleton (Default)**: One instance is shared across the application.
2. **Request Scope**: A new instance is created for each request.
3. **Transient Scope**: A new instance is created every time the provider is injected.

### Example of Scoped Providers:

```typescript
import { Injectable, Scope } from '@nestjs/common';

@Injectable({ scope: Scope.REQUEST }) // A new instance is created for each request
export class RequestScopedService {}
```

## Custom Providers

In some cases, you might want to create custom providers, such as when you want to inject values (e.g., configurations) or dynamically create providers based on conditions.

### Value Provider Example:

```typescript
@Module({
  providers: [
    {
      provide: 'APP_NAME', // Custom provider token
      useValue: 'MyNestApp', // Value to be injected
    },
  ],
})
export class AppModule {}
```

You can then inject this value into your classes using `@Inject()`:

```typescript
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class AppService {
  constructor(@Inject('APP_NAME') private readonly appName: string) {}

  getAppName(): string {
    return this.appName;
  }
}
```

### Factory Provider Example:

You can use a **factory function** to dynamically create a provider.

```typescript
@Module({
  providers: [
    {
      provide: 'CONFIG',
      useFactory: () => ({
        database: process.env.DB_HOST,
      }),
    },
  ],
})
export class AppModule {}
```

## Providers in Global Modules

If a provider needs to be available throughout the entire application (across multiple modules), you can make the module **global** by using the `@Global()` decorator.

Example:

```typescript
import { Global, Module } from '@nestjs/common';

@Global()
@Module({
  providers: [LoggerService],
  exports: [LoggerService],
})
export class LoggerModule {}
```

By making the `LoggerModule` global, the `LoggerService` can be injected into any other module without needing to import `LoggerModule` explicitly.




______________________________________________________


# Exception Handling



**Exception handling** in **NestJS** is a key aspect of building robust and resilient applications. It ensures that errors are properly caught and handled, providing meaningful responses to clients while maintaining the stability of the application. NestJS offers both a **default exception handling mechanism** and the ability to **customize exception handling** as per your needs.

### Default Exception Handling in NestJS

NestJS has built-in support for handling HTTP exceptions. When an error occurs inside a controller or service, NestJS will automatically catch and handle it by returning a **standardized response** to the client.

For example, if a service throws an unhandled error, NestJS catches it and responds with a 500 Internal Server Error:

```typescript
@Get()
findAll() {
  throw new Error('Something went wrong');
}
```

Response:

```json
{
  "statusCode": 500,
  "message": "Internal server error"
}
```

### HTTP Exceptions

NestJS provides a set of predefined **HTTP exceptions** (based on the HTTP status codes) that can be thrown to return specific error responses. These exceptions can be used in services or controllers to explicitly handle certain error conditions.

Some common exceptions include:

- **`BadRequestException`** (400 Bad Request)
- **`UnauthorizedException`** (401 Unauthorized)
- **`ForbiddenException`** (403 Forbidden)
- **`NotFoundException`** (404 Not Found)
- **`InternalServerErrorException`** (500 Internal Server Error)

#### Example: Using HTTP Exceptions

```typescript
import { Controller, Get, NotFoundException } from '@nestjs/common';

@Controller('users')
export class UsersController {
  @Get(':id')
  findOne(@Param('id') id: string) {
    const user = this.findUserById(id);
    if (!user) {
      throw new NotFoundException(`User with ID ${id} not found`);
    }
    return user;
  }

  findUserById(id: string) {
    return null; // Simulate a missing user
  }
}
```

Response:

```json
{
  "statusCode": 404,
  "message": "User with ID 123 not found",
  "error": "Not Found"
}
```

### Customizing the Error Response

You can customize the error response by creating **custom exceptions** that extend NestJS's `HttpException` class.

#### Example: Custom Exception

```typescript
import { HttpException, HttpStatus } from '@nestjs/common';

export class CustomException extends HttpException {
  constructor() {
    super('Custom error message', HttpStatus.BAD_REQUEST);
  }
}
```

You can then use this exception in your controllers or services:

```typescript
@Controller('users')
export class UsersController {
  @Get('custom-error')
  customError() {
    throw new CustomException();
  }
}
```

Response:

```json
{
  "statusCode": 400,
  "message": "Custom error message"
}
```

### Exception Filters

While the default handling is sufficient for many cases, you might need more control over how exceptions are processed and transformed into responses. This is where **exception filters** come into play.

An **exception filter** is a class that implements the `ExceptionFilter` interface. It provides a way to catch exceptions globally or on a per-controller/per-route basis and modify the response.

#### Example: Creating a Global Exception Filter

```typescript
import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';
import { Request, Response } from 'express';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
        message: exception.message,
      });
  }
}
```

### Applying the Exception Filter

Once the exception filter is created, you can apply it at different levels:

- **Globally** (for the entire application)
- **Controller-level**
- **Route-level**

#### Applying Globally

In the `main.ts` file, use `app.useGlobalFilters()` to apply the exception filter across the entire application:

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { HttpExceptionFilter } from './http-exception.filter';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalFilters(new HttpExceptionFilter()); // Apply globally
  await app.listen(3000);
}
bootstrap();
```

#### Applying at the Controller Level

To apply an exception filter to a specific controller, use the `@UseFilters()` decorator:

```typescript
import { Controller, Get, UseFilters } from '@nestjs/common';

@Controller('users')
@UseFilters(HttpExceptionFilter) // Apply to the controller
export class UsersController {
  @Get()
  findAll() {
    throw new HttpException('Something went wrong', 500);
  }
}
```

#### Applying at the Route Level

You can also apply exception filters at the method level:

```typescript
@Controller('users')
export class UsersController {
  @Get()
  @UseFilters(HttpExceptionFilter) // Apply to the specific route
  findAll() {
    throw new HttpException('Something went wrong', 500);
  }
}
```

### Built-in Filters

NestJS provides a built-in `BaseExceptionFilter` that you can extend when creating custom exception filters. This allows you to retain the default exception handling behavior while adding custom logic.

### Catching Non-HTTP Exceptions

You can use the `@Catch()` decorator in your custom exception filter to catch other types of exceptions, such as system errors (e.g., database errors) or custom exceptions.

#### Example: Catching All Exceptions

```typescript
import { ExceptionFilter, Catch, ArgumentsHost } from '@nestjs/common';
import { Request, Response } from 'express';

@Catch() // No parameters means it catches all exceptions
export class AllExceptionsFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    const status = exception instanceof HttpException
      ? exception.getStatus()
      : 500;

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
        message: (exception as any).message || 'Internal server error',
      });
  }
}
```

This filter catches any exception (not just `HttpException`) and returns a response with a 500 status code by default if the exception is not an `HttpException`.

### Built-in Exception Filter for Validation Errors

NestJS provides an automatic exception handling mechanism for **validation errors** (e.g., when using `class-validator` and `class-transformer`). If validation fails (for example, in a DTO), NestJS will throw a `BadRequestException` with a detailed error message.

```typescript
import { IsNotEmpty } from 'class-validator';

export class CreateUserDto {
  @IsNotEmpty()
  name: string;
}
```

When validation fails, NestJS automatically returns a `400 Bad Request` response with the validation error details:

```json
{
  "statusCode": 400,
  "message": ["name should not be empty"],
  "error": "Bad Request"
}
```

### Summary

- **Default Exception Handling**: NestJS automatically handles unhandled exceptions by returning a standardized HTTP response.
- **HTTP Exceptions**: Predefined HTTP exceptions (like `NotFoundException`, `BadRequestException`) can be thrown to handle specific errors.
- **Custom Exceptions**: You can create custom exceptions by extending the `HttpException` class.
- **Exception Filters**: Provide more granular control over error handling. Filters can be applied globally, to controllers, or to specific routes.
- **Validation Errors**: Validation errors in DTOs are automatically handled, returning a `BadRequestException` response.

Using these mechanisms, you can ensure that your application provides meaningful and consistent error responses to clients while keeping your application resilient to unexpected errors.

_____________________



# PIPES


In **NestJS**, **pipes** are used for **data transformation** and **validation**. They play a crucial role in ensuring that incoming data is properly formatted and meets specific requirements before it reaches the route handler or controller. Pipes can be applied to route parameters, the body of requests, and other inputs.

### Key Responsibilities of Pipes:
1. **Transformation**: Pipes can automatically transform input data into the desired format (e.g., converting a string to a number).
2. **Validation**: Pipes can validate incoming data and throw errors if the data is not valid, preventing further execution.

### How Pipes Work
Pipes are executed **before** the route handler is called. They receive the incoming request data, process it (validate/transform), and pass it to the handler if valid. If the data is invalid, the pipe can throw an exception, returning an appropriate error response.

### Built-in Pipes in NestJS
NestJS provides several built-in pipes for common transformation and validation tasks:

1. **`ValidationPipe`**: Used for validating request payloads (especially with DTOs).
2. **`ParseIntPipe`**: Transforms a string parameter to an integer.
3. **`ParseBoolPipe`**: Transforms a string parameter to a boolean.
4. **`ParseArrayPipe`**: Transforms a string to an array.
5. **`DefaultValuePipe`**: Provides a default value if none is provided.

#### Example: `ParseIntPipe`

```typescript
import { Controller, Get, Param, ParseIntPipe } from '@nestjs/common';

@Controller('users')
export class UsersController {
  @Get(':id')
  findOne(@Param('id', ParseIntPipe) id: number) {
    // id will be an integer thanks to ParseIntPipe
    return `User with ID: ${id}`;
  }
}
```

In this example, `ParseIntPipe` converts the `id` parameter from a string to an integer. If the `id` cannot be converted, it throws an error, and NestJS responds with a `BadRequestException`.

### Using `ValidationPipe`

The **`ValidationPipe`** is commonly used with **DTOs (Data Transfer Objects)** to validate incoming request payloads using decorators from the `class-validator` package.

#### Example: Using `ValidationPipe` with a DTO

1. **Install Dependencies**:

```bash
npm install class-validator class-transformer
```

2. **Create a DTO**:

```typescript
import { IsString, IsInt, Min } from 'class-validator';

export class CreateUserDto {
  @IsString()
  name: string;

  @IsInt()
  @Min(1)
  age: number;
}
```

3. **Apply `ValidationPipe`**:

```typescript
import { Controller, Post, Body, ValidationPipe } from '@nestjs/common';
import { CreateUserDto } from './create-user.dto';

@Controller('users')
export class UsersController {
  @Post()
  create(@Body(new ValidationPipe()) createUserDto: CreateUserDto) {
    // Data will be validated before reaching this point
    return `User created: ${createUserDto.name}`;
  }
}
```

In this example:
- The `ValidationPipe` validates the incoming `CreateUserDto`. If the payload is invalid (e.g., missing the `name` field or having a negative age), the pipe will automatically throw a `BadRequestException` with detailed error messages.

### Global, Route, and Parameter-Level Pipes

You can apply pipes at three levels:
1. **Globally**: The pipe is applied to every route in the application.
2. **At the controller or route handler level**: The pipe is applied only to specific routes or controllers.
3. **At the parameter level**: The pipe is applied to a specific parameter of a route.

#### Applying Pipes Globally

In `main.ts`, you can apply a pipe globally:

```typescript
import { ValidationPipe } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe()); // Apply globally
  await app.listen(3000);
}
bootstrap();
```

#### Applying Pipes at the Controller or Route Level

```typescript
import { Controller, Post, Body, ValidationPipe } from '@nestjs/common';
import { CreateUserDto } from './create-user.dto';

@Controller('users')
export class UsersController {
  @Post()
  create(@Body(new ValidationPipe()) createUserDto: CreateUserDto) {
    return `User created: ${createUserDto.name}`;
  }
}
```

#### Applying Pipes at the Parameter Level

```typescript
import { Controller, Get, Param, ParseIntPipe } from '@nestjs/common';

@Controller('users')
export class UsersController {
  @Get(':id')
  findOne(@Param('id', ParseIntPipe) id: number) {
    return `User with ID: ${id}`;
  }
}
```

### Custom Pipes

You can create custom pipes by implementing the `PipeTransform` interface. Custom pipes are useful when you need specialized validation or transformation logic.

#### Example: Creating a Custom Pipe

```typescript
import { PipeTransform, Injectable, BadRequestException } from '@nestjs/common';

@Injectable()
export class UppercasePipe implements PipeTransform {
  transform(value: string) {
    if (typeof value !== 'string') {
      throw new BadRequestException('Value must be a string');
    }
    return value.toUpperCase();
  }
}
```

You can then apply this custom pipe in your controller:

```typescript
import { Controller, Get, Param } from '@nestjs/common';
import { UppercasePipe } from './uppercase.pipe';

@Controller('users')
export class UsersController {
  @Get(':name')
  findOne(@Param('name', UppercasePipe) name: string) {
    return `User: ${name}`;
  }
}
```

In this example, the `UppercasePipe` converts the `name` parameter to uppercase before passing it to the route handler.

### Pipe Options in `ValidationPipe`

The `ValidationPipe` comes with several configurable options for handling how validation is performed:

- **`whitelist`**: Strips out properties that are not defined in the DTO.
- **`forbidNonWhitelisted`**: Throws an error if any properties are not defined in the DTO.
- **`transform`**: Automatically transforms payloads to be instances of the DTO.

#### Example: Using `ValidationPipe` Options

```typescript
import { Controller, Post, Body, ValidationPipe } from '@nestjs/common';
import { CreateUserDto } from './create-user.dto';

@Controller('users')
export class UsersController {
  @Post()
  create(
    @Body(new ValidationPipe({ whitelist: true, forbidNonWhitelisted: true, transform: true }))
    createUserDto: CreateUserDto
  ) {
    return `User created: ${createUserDto.name}`;
  }
}
```

In this example:
- **`whitelist: true`**: Only the properties defined in the DTO (`name` and `age`) will be retained. Any extra properties in the request payload will be stripped.
- **`forbidNonWhitelisted: true`**: If extra properties are present, NestJS will throw an error.
- **`transform: true`**: Automatically transforms the input into an instance of the `CreateUserDto`.

### Summary

- **Pipes** in NestJS are used for data transformation and validation.
- NestJS provides several built-in pipes, such as `ValidationPipe`, `ParseIntPipe`, and `ParseBoolPipe`.
- Pipes can be applied at the global, controller, or parameter level.
- **Custom pipes** can be created by implementing the `PipeTransform` interface.
- The `ValidationPipe` is commonly used with DTOs to validate incoming request payloads, offering options like `whitelist` and `transform`.
  
By using pipes, NestJS ensures that incoming data is well-structured and validated before being processed by the application, making it easier to manage input errors and improve security.


































