## Introducing TinySaas: a library for building multitenant applications in ASP.NET Core

Some years back, I used to maintain a SaaS application built on ASP.NET WebForms. Many organizations are aiming to migrate existing applications built on the ASP.NET Framework to the amazing .NET Core.

TinySaas culminated from my work in migrating an ASP.NET legacy application to ASP.NET Core.

---

# Why TinySaas?

There abound on the internet several frameworks that aim to allow developers to build multitenant applications using the .NET Core Framework, but the keyword (**Framework**).

TinySaas is a library that aims to add multitenancy support to new and existing ASP.NET Core applications without forcing the adoption of a new framework.

The credit for this library goes to [Gunnar](https://gunnarpeipman.com/) and [Michael](https://michael-mckenna.com/), two great authors whose several blog articles on this topic served as the foundation for this simple library.

---

## Getting Started

Multitenancy support should not require a rewrite nor massive changes. With TinyTenant, you can now add multitenancy support to both new and existing projects in simple steps.

1. Add `CodEaisy.TinySaas.AspNetCore` to your application via Nuget or the .NET CLI

```bash
dotnet add package CodEaisy.TinySaas.AspNetCore --version 1.0.0
```

2. In `Startup.cs`, add the following inside the `ConfigureServices` method.

  ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // register all global singleton services here, and also dependencies for your TenantStore and ResolutionStrategy if any

        // ...

        // OPTION 1
        services.AddMultiTenancy<Tenant, TenantStore<Tenant>, TenantResolutionStrategy>();

        // OPTION 2
        // uses default `CodEaisy.TinySaas.Model.TinyTenant` as tenant model
        services.AddMultiTenancy<TenantStore<TinyTenant>, TenantResolutionStrategy>();

        // ...

        // services.AddControllers();
    }
  ```

  Then, add the following in the `Configure` method.

  ```csharp
  public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
  {
      if (env.IsDevelopment())
      {
          app.UseDeveloperExceptionPage();
      }

      // enable multitenant support, with missing tenant handler and tenant container

      // OPTION 1
      // missing tenant handler has a dependency that can be provided immediately
      app.UseMultitenancy<Tenant, MissingTenantHandler, MissingTenantOptions>(missingTenantOptions);

      // OPTION 2
      // missing tenant handler does not have a dependency or dependency is already registered in services
      app.UseMultitenancy<Tenant, MissingTenantHandler>();

      // OPTION 3
      // Use `SimpleTenant` as tenant model, and missing tenant handler does not have a dependency or dependency is already registered in the services

      app.UseMultitenancy<TMissingTenantHandler>()

      // ...
  }
  ```

3. In `Program.cs`, add the following in the `CreateHostBuilder` method.

  ```csharp
  public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            })
            // OPTION 1: add multitenant support via TenantStartup class
            .ConfigureMultiTenancy<TenantStartup, Tenant>();
            // OPTION 2: add multitenant support via static method
            .ConfigureMultiTenancy<Tenant>(ClassName.StaticMethodName);
  ```

**NOTE**:

- `Tenant` must implement `CodEaisy.TinySaas.Interface`  `ITenant`.
- `TenantStore` must implement `CodEaisy.TinySaas.Interface.ITenantStore`.
- `TenantResolutionStrategy` must implement `CodEaisy.TinySaas.Interface.ITenantResolutionStrategy` respectively.
- `TenantStartup` must implement `IMultiTenantStartup`
- `ClassName.StaticMethodName` must be of type `System.Action<TTenant, Autofac.ContainerBuilder>` where `TTenant` implements `ITenant`

---

## Use Cases

TinySaas supports the most common use cases for multitenant development, and below are some examples.

### Global Services

Global Services are services that do not have a core dependency on the tenant information, and they can be registered just the same way you have been used to in .NET Core, in the `Startup.ConfigureServices` method.

```csharp
public void ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddSingleton<IGlobalSingletonService, GlobalSingletonService>();

services.AddScoped<IGlobalScopedService, GlobalScopedService>();

services.AddTransient<IGlobalTransientService, GlobalTransientService>();

        // add multitenancy support here

        services.AddControllers();
    }
```

### Tenant Services

Tenant Services are services that have core dependencies on the tenant information. TinySaas allows you to register these services like every other .NET Core service but in a tenant aware location.

Usually, tenant services will be registered either in the `ConfigureServices` method of your `TenantStartup` class or in the delegate passed to the `IHostBuilder.ConfigureMultiTenancy` in the `Program.cs` file.

```csharp
    public void ConfigureTenantServices(Tenant tenant, ContainerBuilder container)
    {
        // you can either register services using the provided container builder

        // tenant singleton
        container.RegisterType<TenantSingletonService>().SingleInstance();

        // tenant scoped
        container.RegisterType<TenantScopedService>().InstancePerRequest();

        // tenant transient
        container.RegisterType<TenantTransient>().InstancePerDependency();

        // basically, you can use all the dependency injection features provided by Autofac on the container.

        #region .NET Core DI way
        // you can also do it in the well-known .NET Core default DI way

        var services = new ServiceCollection();

        // tenant singleton
        services.AddSingleton<ITenantSingletonService, TenantSingletonService>();

        // tenant scoped
        services.AddScoped<ITenantScopedService, TenantScopedService>();

        // tenant transient
        services.AddTransient<ITenantTransientService, TenantTransientService>();

        // THIS IS VERY IMPORTANT IF YOU USE THE SERVICE COLLECTION TO REGISTER ANY SERVICES HERE.
        container.Populate(services);
        
        #endregion

    }
```

### Databases

Multitenancy is not complete without databases, and TinySaas has an answer to all your needs.

#### Shared Database (Single Schema and Schema per Tenant)

Register your `DbContext` the old way and inject `IHttpContextAccessor` via your DbContext constructor.

```csharp
// Startup.cs class
public void ConfigureService(IServiceCollection services)
{
    //....
    services.AddDbContext<AppDbContext>(...);
    //...
}

// AppDbContext.cs
public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options, IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
        
        // to get the tenant information in any method
        // var tenant = _httpContextAccessor.HttpContext.GetCurrentTenant<TTenant>();
    }
}
````

#### Database Per Tenant
Simply move the `DbContext` registration to the multitenant service configuration method.

```csharp
    public void ConfigureTenantServices(Tenant tenant, ContainerBuilder container)
    {
         // assume you had connectionString as a property on the tenant, you can easily do
        var services = new ServiceCollection();
        services.AddDbContext<AppDbContext>(tenant.ConnectionString);

        container.Populate(services);
    }
```

### Other Concerns
`Options` can also be registered the same way as discussed for services and databases.


You can check out the source repo for TinySaas on Github, and give feedback via the issues tab.

Thank you for reading 🍾.
---