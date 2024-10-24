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
   ## Step 8: Test Your Application

1. **Run the Application**:
   - Use the built-in development server by pressing `F5` or clicking the Start button in Visual Studio.
   - The application will launch in your default web browser.

2. **Create Users**:
   - Manually add users with different roles in your database (using a database management tool) or implement a registration feature.

3. **Check Role Access**:
   - Navigate to the various pages (Admin, Manager, User).
   - Verify that:
     - Admin users can access the Admin page.
     - Manager users can access the Manager page.
     - User accounts can access the User page.
   - Ensure unauthorized users are redirected or shown a message when trying to access restricted pages.

## Step 9: Improve and Deploy

1. **Enhance the UI**:
   - Consider using Bootstrap or Tailwind CSS for better styling and responsiveness.
   - Create a layout in `Shared/MainLayout.razor` to standardize your pages.
   - Use components to avoid duplication and make the app easier to maintain.

2. **Deploy Your Application**:
   - Choose a hosting platform (e.g., Azure, IIS, or any cloud provider).
   - Follow the deployment instructions specific to your chosen platform.
   - For Azure:
     - Create an App Service in the Azure Portal.
     - Publish your application directly from Visual Studio by right-clicking the project and selecting **Publish**.
   - Ensure your connection strings and settings are configured correctly in the production environment.

## Conclusion

This guide provides a foundational framework for building a Blazor web application with role-based access control. Each step can be expanded upon based on specific project requirements. If you have any questions or need further assistance, feel free to ask!

