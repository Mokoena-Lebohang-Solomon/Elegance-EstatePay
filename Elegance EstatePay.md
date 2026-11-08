# Elegance EstatePay

Elegance EstatePay is a two-part estate-services and payment-tracking system: a native Android client and an ASP.NET Core web application with a JSON API. Both clients use the web/API service for authentication and operational data; neither client connects directly to SQL Server.

## Project files

- **Android app:** `EleganceEstatePay-Android-API28.zip` — Android Studio project, Java, minimum/target API 28, launcher name `eleganceestatepay`, branded adaptive launcher icon, bearer-token API client, and resident/officer/administrator workflows.
- **Web app and API:** `EleganceEstatePay-Web-Updated.zip` — Visual Studio / ASP.NET Core MVC project, mobile API adapter, EF Core SQL Server integration and database schema assets.

Extract each archive into its own folder before opening it with Android Studio or Visual Studio. These archives contain source projects, not a signed Android APK or a published web service.

## Technologies

- **Android:** Java 11 language level, Android Gradle Plugin, AndroidX AppCompat, Material Components, Android API 28 minimum and target, `HttpURLConnection` REST client, JSON, and Android SharedPreferences for optional remembered bearer tokens.
- **Web/API:** ASP.NET Core MVC on .NET 9, C#, controller-based REST endpoints, bearer-token authentication for mobile API calls, cookie authentication for browser pages, Entity Framework Core 9, and Microsoft SQL Server.
- **Database:** SQL Server settings are read by the web application. Keep database credentials in .NET User Secrets during local development or deployment environment secrets; never add passwords, Firebase service-account keys, or other secrets to Git.

## Android setup

1. Extract `EleganceEstatePay-Android-API28.zip` and open the project root in Android Studio.
2. Let Gradle sync and set the API base URL for the web app you have deployed. The project defaults to `http://eleganceestatepay.runasp.net/`; override it with `-PestatePayApiBaseUrl=https://your-api-host/` when building. Use HTTPS for real accounts.
3. Build and install the `app` module. The Android app needs a reachable running/deployed web API; a SQL Server host name is not an API URL.

## Web/API setup

1. Extract `EleganceEstatePay-Web-Updated.zip` and open `EleganceEstatePay.slnx` or the project in Visual Studio 2022 with the .NET 9 SDK.
2. Configure `ConnectionStrings__DefaultConnection` as a deployment secret, or configure the SQL password with .NET User Secrets as described in the included web README. The checked-in configuration intentionally does not contain a database password.
3. Build and run or deploy the web app. Configure the Android base URL to the deployed API origin. The mobile adapter uses the same EF Core database, accounts, roles and service/payment records as the web app.

## API and app behavior

Mobile API routes include authentication and token refresh, service/tariff listing, resident profile/payment/notifications, officer payment and resident-account workflows, administrator management, reports, and review features. The Android screens use authenticated API calls for operational data. See the Android and web project READMEs for endpoint and role details.

The current notification feature is **in-app notification history** stored by the API and displayed when the app fetches its notification screen. **Firebase Cloud Messaging (FCM) push delivery is not configured in these source archives yet.** To enable phone push, the app owner must create a Firebase project, register the Android package `com.example.mohale`, provide its `google-services.json`, and configure a Firebase service-account credential as a server-side secret; the web API must then register device tokens and send FCM messages when it creates a notification. Do not commit either Firebase credential file or service-account secret. Until that setup and implementation are completed, notifications appear when the user opens/refreshes the app; they are not guaranteed to arrive as operating-system push alerts.

## Important deployment notes

- Use HTTPS before entering real passwords or using bearer tokens. Do not expose a Development-only HTTP endpoint publicly.
- Android calls the ASP.NET API; the API calls SQL Server. Do not place SQL credentials in the Android app.
- The card workflow records and validates a payment request; it does not itself charge a bank card. Use an appropriate PCI-compliant payment provider for live card processing.
- Review the supplied web README and deployment settings before publishing. Change any development/demo credentials before production use.
