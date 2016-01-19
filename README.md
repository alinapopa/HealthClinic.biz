# HealthClinic.biz #

During Connect(); //2015 we showcased many technologies available to you as a developer across Azure, Office, Windows, Visual Studio and Visual Studio Team Services. We’ve also heard from you that you love to have real-world applications through which you can directly experience what’s possible using those technologies. This year, then, we built out a full health and technology scenario for our Connect(); //2015 demos and are delighted to share all the source code with you.

HealthClinic.biz is a fictitious regular doctor practice specialized in offering healthcare preventive care. This clinic is using different Microsoft and multi-channel apps built with Visual Studio 2015 to grow their business and modernize their customer experience. They also innovate and offer multiple apps and services—including websites, mobile apps, and wearable apps—to empower their patient’s well-being with easy access to manage their healthcare data and stay healthy.

# Licenses #
This project uses some third-party assets with a license that requires attribution:

 - **Roboto Font**: by [Christian Robertson](https://plus.google.com/110879635926653430880/about) ([Roboto at Google Fonts](https://www.google.com/fonts/specimen/Roboto))
 - **Raleway Font**: by Matt McInerney, Pablo Impallari, Rodrigo Fuenzalida and by Igino Marini  ([Raleway at Google Fonts](https://www.google.com/fonts/specimen/Raleway))

For extra information about licenses, you can see it at the dependency repositories.

# Prerequisites #

 - Windows 10
 - Visual Studio 2015 Update 1 (make sure you install UWP Development tools, Cross Platform Mobile Development, and Office Developer Tools).
 - [Xamarin for Visual Studio](https://xamarin.com/visual-studio)
 - [Microsoft Azure SDK](http://go.microsoft.com/fwlink/?LinkId=617168) for Visual Studio 2015
 - Microsoft Office 2016
 - Bing Maps Key - [Getting a Bing Maps Key](https://msdn.microsoft.com/en-us/library/ff428642.aspx)
 - Microsoft [Azure subscription](https://azure.com/)

# Blog posts #
Here's links to blog posts related to this project:

 - [Connect(“demos”); // 2015: HealthClinic.biz](http://blogs.msdn.com/b/visualstudio/archive/2015/12/08/connect-demos-2015-healthclinic-biz.aspx) by Erika Ehrli
 - [ASP.NET 5 and .NET Core RC1 in context Plus all the Connect 2015 News](http://www.hanselman.com/blog/ASPNET5AndNETCoreRC1InContextPlusAllTheConnect2015News.aspx) by Scott Hanselman

# Setup and run instructions #
**Public and private websites:**
Open solution 01_Demos_ASPNET5. Set MyHealth.Web project as startup project and run. The application will start running locally on IISExpress.
The private website requires credentials for access; the default credentials can be viewed or changed in [src\MyHealth.Web\appsettings.json](https://github.com/Microsoft/HealthClinic.biz/blob/master/src/MyHealth.Web/appsettings.json).

If you have an Azure account you can publish the web app to the cloud. Right-click on MyHealth.Web project, select Publish. Choose "Microsoft Azure Web Apps" as a publish target. Enter your credentials to connect to your Azure account and select the subscription to use. Select New web app. Create a new AppService plan, ResourceGroup and Database server if needed. Click "Publish" button.

The private website is set to use the local database in appsettings.json. To change it to use the database in Azure, replace this part of the connection string 
```Server=(localdb)\\mssqllocaldb;Database=MyHealth;```
with your database in Azure:  
```Server=<your db server>.database.windows.net;Database=<db name>;User Id=<user id>;Password=<your password>;```

You can use SQL Server 2014 Management Studio to examine the database you have created, see the following [instructions] (https://azure.microsoft.com/en-us/documentation/articles/sql-database-manage-azure-ssms/).

**Integration with Office 365**
To set up the integration with Office 365, the web app must be added to Azure Active Directory through the Azure Portal and authorizations configured; the mobile apps (iOS and Android app) must be registered to Active Directory at https://apps.dev.microsoft.com.
Use the Azure Portal to [register the web application] (https://azure.microsoft.com/en-us/documentation/articles/mobile-services-how-to-register-active-directory-authentication/#registering-your-app) in Azure Active Directory under your Organization. From Azure Portal, configure the application's permissions: go to 'Configure' tab, then 'Permissions to other applications' -> Add Application -> Microsoft Graph. Under 'Application Permissions' check 'Read calendars in all mailboxes'.
Read the following [documentation] (https://msdn.microsoft.com/en-us/office/office365/howto/building-service-apps-in-office-365) to learn how to create a certificate and access token for the application. Add the certificate (.pfx file) under folder src\MyHealth.Web\content. In src\MyHealth.Web\appsettings.json, set "ClientCertificatePfx" value to your certificate file name. Also in the same appsettings.json file, set "ClientId" to the value you get from Azure Portal. Create a key in Azure Portal and fill the "ClientKey" value in json file. Also fill the values for "ClientCertificatePassword" and "DebugTenantId".

User the following [link] (https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-app-registration/) to learn how to register the iOS and Android application to Azure Directory. Then fill the DroidClientId and iOSClientId in file src\MyHealth.Client.Core\AppSettings.cs with the ID's obtained at registration.