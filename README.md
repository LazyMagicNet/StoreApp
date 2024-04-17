﻿# PestApp 

This solution contains a sample cross platform application supporting iOS, Android, Windows, MacOS and Web apps. 
This client app uses Model View ViewModel (MVVM) architecture and is built using the LazyStackClient framework. 
- The View is defined in a Razor Class Library (RCL) and shared between the Web and MAUI apps.
- The ViewModels are defined in a .NET C# library and shared between the Web and MAUI apps.
- Data Transfer Objects (DTOs) are defined in the StoreClientSchema namespace in the StoreClientSDK package. This package is published by the  Service project. This package also contains the StoreClientSDK namespace which contains the client code to call the Store API.
- Model partial classes are generated by the LazyStack.LzItemViewModelGenerator Analyzer. These partial classes can be enhanced by the developer with additional properties, methods.
- Model Validation partial classes are generated by the LazyStack.LzItemViewModelGenerator Analyzer. These partial classes can be enhanced by the developer with validation using the FluentValidation library. 

## Projects
| Project | Description |
| :--- | :--- |
| WASMApp | Blazor WebAssembly app. SPA/PWA |
| MAUIApp | .NET MAUI Hybrid app - iOS, Android, Windows, MacOs. Uses WebView to render UI. |
| BlazorUI | .NET C# Razor Class Library containing the shared UI components and pages for the Web and MAUI apps |
| ViewModels | .NET C# Library containing the shared ViewModels for the Web and MAUI apps |
| Tenancy | .NET project containing tenancy assets for local development. |
| Config | .NET project containing config for local development. |

## Libraries
|Library | Description |
| :--- | :--- |
| LZMServiceClientSDK | Package generated by the Service solution that supports calls to the Store API. It also contains the ServiceClientSDK and ServiceSchema namespace with the DTO classes. |
| LazyStack.Blazor | Razor Class library containing shared UI components |
| LazyStack.ViewModels | .NET C# Library containing reusable ViewModel generic classes and interfaces |
| LazyStack.Auth | .NET C# library providing authentication, authorization, and connection for the Store API |

## Analyzers 
.NET Analyzers are used to generate code. 

| Analyzer | Description |
| :--- | :--- |
| LazyStack.LzItemViewModelGenerator | Generates a Model from a DTO. |
| LazyStack.DIHelper | Generate a DIHelper class with a RegisterServices() method to automatically register classes which implement ILzTransient, ILzSingleton or ILzScoped interfaces. |
| LazyStack.FactoryGenerator | Generate DI factory class code for classes annotated with [Factory] annotation. This avoids a boatload of boilerplate coding. This analyzer is primarily used with ViewModel classes.|


## MAUI Hybrid / WASM App Notes
The MAUI app is a hybrid app that uses a WebView to render the UI. The Microsoft template for Hybrid apps and WASM apps must be modified slightly to share the common Store.Blazor Razor Class Library (RCL) containing the UI components and pages.

Both the MAUI and WASM projects are configured to be a "thin" as possible to avoid duplication of code. 

## MVVM Architecture
The Model View ViewModel architecture is implemented using the LazyStack framework. 

| Layer | Description |
| :--- | :--- |
| Deployment | WASMApp, MAUIApp |
| Views | Blazor components and pages are defined in the BlazorUI project and use the LazyStack.Blazor library. |
| ViewModels |  ViewModels are defined in the ViewModels project and use the LazyStack.ViewModels library. |
| Model | Models are derived from Data Transfer Objects (DTOs). DTOs are defined and processed in the StoreSchema and StoreClientSDK libraries. These libraries are generated by the LazyStackMDD tool.


Model-View-ViewModel (MVVM) is a software architectural pattern primarily used in building user interfaces. Here's a brief description of its three main components:

1. **Model**: Represents the data of the application and is independent of the user interface and does not contain any information about how the data is presented. We generate a partial Model class that derived from the Data Transfer Object (DTO) exchanged with the Store API. Use this class to add additional properties and methods to those provided in the DTO. We also generate partial ModelValidation class for the Model. Use this class to add Validation using the FluentValidation library. 

2. **View**: The user interface of the application. The View displays data from the ViewModel and sends user commands (like button clicks) to the ViewModel. The View is usually designed in a declarative manner, focusing on the layout and visual elements. Our View classes inherit from LzCoreComponentBase<T> which provides a robust view management feature set and uses the ReactiveUI framework to handle complex data flows and change propagation in applications.

3. **ViewModel	**: Acts as an intermediary between the View and the Model. It provides data from the Model in a form that the View can easily use, usually by implementing properties and commands to which the View can bind. The ViewModel handles the presentation logic and user interactions, and notifies the View of any changes in the Model. Our ViewModels inherit from LzItemViewModelBase<T>, and LzItemsViewModelBase<T>, which together provide robust CRUDL features based on the ReactiveUI framework. These classes use the StoreClientSDK to call the Store API.

The key advantage of MVVM is that it provides a clear separation of concerns, which improves the maintainability and testability of the application. It also enables more efficient and clean data bindings for dynamic user interfaces, particularly in technologies like WPF, Xamarin, and other XAML-based platforms. ReactiveUI is used to handle complex data flows and change propagation in applications and to blend the Blazor binding model with the MVVM pattern.

## XAML/MAUI
It is possible to replace the Blazor View with a XAML View. The XAML View would be defined in a .NET MAUI project. The XAML View would use the same ViewModels defined in the ViewModels project. The XAML View would use the same Models defined in the StoreSchema and StoreClientSDK libraries. In my experience, the Blazor View approach is sufficient for most applications and provides the opportunity to field the application as both a Web app and a MAUI app as demonstrated in this sample application. A UI built in XAML would only be usable in the MAUI application.

### ReactiveUI
ReactiveUI is an open-source framework for .NET that facilitates the development of reactive, model-view-viewmodel (MVVM) oriented applications. At its core, ReactiveUI is built upon Reactive Extensions (Rx). Rx is a library for composing asynchronous and event-based programs using observable sequences and LINQ-style query operators. ReactiveUI leverages this to handle complex data flows and change propagation in applications. Using ReactiveUI makes it easy to blend the Blazor binding model with the MVVM pattern. 

## LazyStack Libraries
### LazyStack.Base Library
Various base classes and interfaces used throughout the LazyStack framework.

### LazyStack.ViewModels Libraries
| Library | Description |
| :--- | :--- |
| LazyStack.ViewModels | Classes and interfaces for ViewModels |
| LazyStack.ViewModels.Auth | Classes and interfaces for ViewModels using LazyStack.Auth|
| LazyStack.ViewModels.Auth.Notifications | Classes and interfaces for ViewModels using LazyStack.Auth.Notifications |

LazyStack ViewModels provide a robust view management feature set. See the Library documentation for details.

### LazyStack.Blazor Library 

| Library | Description |
| :--- | :--- |
| LazyStack.Blazor | Blazor components and pages |
| LazyStack.Blazor.Auth | Blazor Auth component that works with LazyStack.Auth |
| LazyStack.BlazorSvg | Blazor component that wraps a subset of the JavaScript snap.svg library. |

### LazyStack.Auth Library
| Library | Description |
| :--- | :--- |
| LazyStack.Auth | Authentication, authorization, and connection library. |
| LazyStack.Auth.Cognito | AWS Cognito AuthProvider and ILzHttpClient. |

### LazyStack Analyzers 
| Analyzer | Description |
| :--- | :--- |
| LazyStack.LzItemViewModelGenerator | Generates a Model from a DTO. |
| LazyStack.DIHelper | Generate a DIHelper class with a RegisterServices() method to automatically register classes which implement ILzTransient, ILzSingleton or ILzScoped interfaces. |
| LazyStack.FactoryGenerator | Generate DI factory class code for classes annotated with [Factory] annotation. This avoids a boatload of boilerplate coding. This analyzer is primarily used with ViewModel classes.|

