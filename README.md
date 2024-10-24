# Blazor Web App with Role-Based Access Control

## Overview

This guide provides step-by-step instructions to create a Blazor web application using Visual Studio 2022 with three levels of access: Admin, Manager, and User.

## Prerequisites

- Visual Studio 2022
- ASP.NET and web development workload installed

## Step 1: Set Up Your Environment

1. **Install Visual Studio 2022**:
   - Ensure you have Visual Studio 2022 installed with the ASP.NET and web development workload.

## Step 2: Create a New Blazor WebAssembly Project

1. **Open Visual Studio**.
2. **Select "Create a new project."**
3. **Choose "Blazor WebAssembly App"**.
4. Click **Next**.
5. **Name your project** (e.g., `BlazorAccessControlApp`).
6. Choose a location and click **Create**.
7. **Select the following options**:
   - Check "ASP.NET Core hosted" (if you want a server-side API).
   - Click **Create**.

## Step 3: Set Up User Roles

1. **Add a simple authentication mechanism**. You can use ASP.NET Identity or a simpler approach for demonstration.
2. **Install NuGet Packages**:
   - Right-click on the project in Solution Explorer and select **Manage NuGet Packages**.
   - Install `Microsoft.AspNetCore.Identity` and `Microsoft.AspNetCore.Authentication.JwtBearer`.

## Step 4: Create Models and Role Management

1. **Create User and Role Models**:
   - In the `Shared` project, create a new folder called `Models` and add classes:
     ```csharp
     public class UserRole
     {
         public const string Admin = "Admin";
         public const string Manager = "Manager";
         public const string User = "User";
     }
     ```

2. **Set Up Database Context**:
   - Create a `Data` folder and add a class `AppDbContext` that inherits from `IdentityDbContext`.
   - Configure this in `Startup.cs`.

## Step 5: Create Authentication and Authorization

1. **Configure Services**:
   - In `Server/Startup.cs`, add:
     ```csharp
     services.AddDbContext<AppDbContext>(options => options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));
     services.AddIdentity<IdentityUser, IdentityRole>()
             .AddEntityFrameworkStores<AppDbContext>()
             .AddDefaultTokenProviders();
     ```

2. **Configure JWT Authentication** (optional):
   - Set up JWT authentication in `Startup.cs` if needed.

## Step 6: Create Login and Role-Based Components

1. **Create Login Component**:
   - In `Client/Pages`, create `Login.razor` with a simple form for user login.
   - Handle authentication using `SignInManager`.

2. **Create Admin, Manager, and User Components**:
   - Create three separate pages/components: `Admin.razor`, `Manager.razor`, `User.razor`.
   - Use `[Authorize(Roles = UserRole.Admin)]` or similar for access control.

## Step 7: Set Up Routing

1. **Configure Routing** in `App.razor`:
   ```razor
   <Router AppAssembly="@typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
           <CascadingAuthenticationState>
               <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
           </CascadingAuthenticationState>
       </Found>
       <NotFound>
           <LayoutView Layout="@typeof(MainLayout)">
               <p>Sorry, there's nothing at this address.</p>
           </LayoutView>
       </NotFound>
   </Router>
