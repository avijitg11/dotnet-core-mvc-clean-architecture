# SummerWindsResorts - Clean Architecture Web App (.NET 8)

A modular resort management web application built with **.NET 8, ASP.NET Core MVC, Entity Framework Core, and SQL Server**, following **Clean Architecture** principles and Domain-Driven Design (DDD). This project demonstrates enterprise-level separation of concerns, dependency injection, and scalable business logic implementation.

## 🏗️ Architecture Overview

The project follows **Clean Architecture** with four distinct layers:

- **Domain Layer** (`SummerWindsResorts.Domain`) - Core business entities and logic
- **Application Layer** (`SummerWindsResorts.Application`) - Business rules, services, and contracts
- **Infrastructure Layer** (`SummerWindsResorts.Infrastructure`) - Data access, persistence, and external services
- **Presentation Layer** (`SummerWindsResorts`) - ASP.NET Core MVC (Controllers, Views, ViewModels)

### Stack

- **Framework:** .NET 8, ASP.NET Core MVC
- **Database:** SQL Server with Entity Framework Core 8.0
- **ORM:** Entity Framework Core with migrations
- **Authentication:** ASP.NET Core Identity
- **Payment Processing:** Stripe.NET (v42.9.0)
- **Document Generation:** Syncfusion (DocIO, PDF, Presentation renderers)
- **Frontend:** jQuery AJAX Unobtrusive (for AJAX calls)

## 📁 Project Structure

```
SummerWindsResorts/
├── SummerWindsResorts.Domain/           Core entities and business logic
│   └── Entities/
│       ├── Amenity.cs                   Resort amenities
│       ├── ApplicationUser.cs            Identity user (extends IdentityUser)
│       ├── Booking.cs                    Guest booking records
│       ├── Villa.cs                      Villa/accommodation types
│       └── VillaNumber.cs                Individual villa unit numbers
│
├── SummerWindsResorts.Application/      Business rules and service interfaces
│   ├── Common/                          Cross-cutting concerns
│   ├── Contract/                        DTOs and service contracts
│   └── Services/                        Business logic implementations
│       ├── Interface/                   Service abstractions
│       └── Implementation/
│           ├── VillaService
│           ├── VillaNumberService
│           ├── AmenityService
│           ├── BookingService
│           ├── DashboardService
│           ├── PaymentService
│           └── EmailService
│
├── SummerWindsResorts.Infrastructure/   Data access and external services
│   ├── Data/                            EF Core DbContext and migrations
│   │   ├── ApplicationDbContext.cs       Database configuration
│   │   └── DbInitializer.cs             Seed data initialization
│   ├── Repository/                      Generic repository and Unit of Work
│   ├── Emails/                          Email service implementation
│   └── Migrations/                      Database schema migrations
│
└── SummerWindsResorts/                  ASP.NET Core MVC Presentation
    ├── Controllers/
    │   ├── AccountController.cs         User authentication (Login/Register)
    │   ├── HomeController.cs            Public landing pages
    │   ├── VillaController.cs           Villa management
    │   ├── VillaNumberController.cs     Villa unit management
    │   ├── AmenityController.cs         Amenity management
    │   ├── BookingController.cs         Booking management and Stripe payment
    │   └── DashboardController.cs       Admin dashboard
    ├── ViewModels/                      View-specific data models
    ├── Views/                           Razor templates
    ├── wwwroot/                         Static assets (CSS, JS)
    ├── appsettings.json                 Configuration
    └── Program.cs                       Dependency injection & middleware setup
```

## 🚀 Getting Started

### Prerequisites
- .NET 8 SDK
- SQL Server (local or cloud)
- Visual Studio 2022 / Visual Studio Code

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone https://github.com/avijitg11/dotnet-core-mvc-clean-architecture.git
   cd dotnet-core-mvc-clean-architecture
   ```

2. **Configure the database connection**
   - Open `SummerWindsResorts/appsettings.json`
   - Update the `DefaultConnection` string to point to your SQL Server instance:
   ```json
   "ConnectionStrings": {
     "DefaultConnection": "Server=YOUR_SERVER;Database=SummerWindsResorts;Trusted_Connection=true;"
   }
   ```

3. **Configure external services**
   - Add your **Stripe Secret Key** in `appsettings.json`:
   ```json
   "StripeSecretKey": "your_stripe_secret_key_here"
   ```
   - Add your **Syncfusion License Key**:
   ```json
   "SyncfusionLicensekey": "your_syncfusion_license_here"
   ```

4. **Apply database migrations**
   ```bash
   cd SummerWindsResorts
   dotnet ef database update
   ```

5. **Run the application**
   ```bash
   dotnet run
   ```
   - Application will be available at `https://localhost:5001` (or specified port)

6. **Database seeding**
   - The application automatically seeds initial data (villas, amenities, users) on startup via `DbInitializer`

## ✨ Key Features

### Villa Management
- View available villas with pricing and amenities
- Admin: Create, update, and delete villa types
- Attach amenities to villas

### Booking System
- Guest booking with date range selection
- Automatic villa availability checking
- Booking status tracking (Pending, Completed, Cancelled)

### Payment Processing
- Stripe integration for secure payments
- Payment status tracking and receipts

### Admin Dashboard
- Overview of bookings, revenue, and occupancy
- User activity metrics
- Villa performance analytics

### User Management
- ASP.NET Core Identity authentication
- Role-based access control
- User profile management

### Email Notifications
- Automated booking confirmations
- Payment receipts
- Admin notifications

### Document Generation
- Invoice generation (Word, PDF)
- Booking confirmations (PDF)
- Report exports

## 🔧 Dependency Injection & Services

The `Program.cs` registers all services:

```c#
builder.Services.AddScoped<IUnitOfWork, UnitOfWork>();           // Data access pattern
builder.Services.AddScoped<IVillaService, VillaService>();       // Villa business logic
builder.Services.AddScoped<IBookingService, BookingService>();   // Booking management
builder.Services.AddScoped<IPaymentService, PaymentService>();   // Stripe payments
builder.Services.AddScoped<IEmailService, EmailService>();       // Email notifications
builder.Services.AddScoped<IDashboardService, DashboardService>(); // Analytics
```

## 🗄️ Database

**EF Core Code-First approach** with migration support:
- `ApplicationDbContext` manages all entity mappings
- Migrations stored in `SummerWindsResorts.Infrastructure/Migrations/`
- Identity tables for user authentication
- Relationships: Villa ↔ VillaNumber, Booking ↔ Villa, etc.

## 🔐 Security

- ASP.NET Core Identity for authentication
- Role-based authorization
- HTTPS enforced in production
- Sensitive configuration via `appsettings.json` and environment variables
- HSTS headers enabled

## 📝 Entity Relationships

- **Villa** → multiple **VillaNumbers** (units)
- **Villa** ↔ **Amenities** (many-to-many)
- **Booking** → **Villa** & **ApplicationUser**
- **User** → multiple **Bookings**

## 🛠️ Build & Deploy

### Local Development
```bash
dotnet build
dotnet run --project SummerWindsResorts/SummerWindsResorts.csproj
```

### Production
```bash
dotnet publish -c Release -o ./publish
```
- Deploy to Azure, AWS, or on-premises servers
- Ensure SQL Server is accessible
- Configure environment variables for secrets

## 📚 Learn More

- [Clean Architecture by Robert C. Martin](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- [ASP.NET Core MVC Documentation](https://learn.microsoft.com/en-us/aspnet/core/)
- [Entity Framework Core](https://learn.microsoft.com/en-us/ef/core/)
- [Stripe Documentation](https://stripe.com/docs)

## 📄 License

This project is open source. Feel free to use, modify, and distribute as needed.

## 👨‍💻 Author

**Avijit Ghosh**  
[GitHub Profile](https://github.com/avijitg11)

---

**Happy coding!** If you have questions or suggestions, feel free to open an issue or pull request.
