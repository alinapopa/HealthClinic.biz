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
The private website requires credentials for access; the default username and password can be viewed or changed in [src\MyHealth.Web\appsettings.json](https://github.com/Microsoft/HealthClinic.biz/blob/master/src/MyHealth.Web/appsettings.json).

If you have an Azure account you can publish the web app to the cloud. Right-click on MyHealth.Web project, select Publish. Choose "Microsoft Azure Web Apps" as a publish target. Enter your credentials to connect to your Azure account and select the subscription to use. Select New web app. Create a new AppService plan, ResourceGroup and Database server if needed. Click "Publish" button.

The private website is set to use the local database in appsettings.json. To change it to use the database in Azure, replace this part of the connection string: 

```
Server=(localdb)\\mssqllocaldb;Database=MyHealth;
```
with your database in Azure: 
 
```
Server=<your db server>.database.windows.net;Database=<db name>;User Id=<user id>;Password=<your password>;
```

You can use SQL Server 2014 Management Studio to examine the database you have created, see the following [instructions] (https://azure.microsoft.com/en-us/documentation/articles/sql-database-manage-azure-ssms/).

**Integration with Office 365**

The Office 365 integration allows the patient to find available appointment spots in the doctor's Office 365 calendar; when a new appointment is created from the patient app, it will appear in both the patient's calendar and in the doctor's calendar. This is done using [Microsoft Graph API](http://graph.microsoft.io/). You can watch an introduction to Microsoft Graph API in this Connect(); [video] (https://channel9.msdn.com/Events/Visual-Studio/Connect-event-2015/301).

The web app can access all doctors' Office 365 calendars. The patient app (iOS and Android Xamarin apps; the UWP patient app is not integrated with Office 365) can access the patient Office 365 calendar. The patient app gets the doctor availability info from the web app. When a new appointment is created, the patient app updates the patient calendar, and the web app updates the doctor's calendar.

To set up the integration with Office 365, the web app (ASP.NET app) must be added to Azure Active Directory through the Azure Portal and authorizations configured; the mobile apps (iOS and Android app) must be registered to Active Directory at https://apps.dev.microsoft.com.
Use the Azure Portal to [register the web application] (https://azure.microsoft.com/en-us/documentation/articles/mobile-services-how-to-register-active-directory-authentication/#registering-your-app) in Azure Active Directory under your Organization. From Azure Portal, configure the application's permissions: go to _Configure_ tab, then _Permissions to other applications_ > _Add Application_ > _Microsoft Graph_. Under _Application Permissions_ check _Read calendars in all mailboxes_.
Read the following [documentation] (https://msdn.microsoft.com/en-us/office/office365/howto/building-service-apps-in-office-365) to learn how to create a certificate and access token for the application. Add the certificate (.pfx file) under folder [src\MyHealth.Web\content](https://github.com/Microsoft/HealthClinic.biz/tree/master/src/MyHealth.Web/content). In [src\MyHealth.Web\appsettings.json](https://github.com/Microsoft/HealthClinic.biz/blob/master/src/MyHealth.Web/appsettings.json), set the "ClientCertificatePfx" value to your certificate file name. Also in the same appsettings.json file, set "ClientId" to the value you get from Azure Portal. Create a key in Azure Portal and fill the "ClientKey" value in json file. Also fill the values for "ClientCertificatePassword" and "DebugTenantId".

User the following [link] (https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-app-registration/) for instructions to register the iOS and Android application to Azure Directory. Then fill the DroidClientId and iOSClientId in file [src\MyHealth.Client.Core\AppSettings.cs](https://github.com/Microsoft/HealthClinic.biz/blob/master/src/MyHealth.Client.Core/AppSettings.cs) with the ID's obtained at registration.

**WPF app**

The WPF app is the receptionist app, which allows the receptionist to see patients' data, upcoming appointments, and create new appointments. The WPF app code is in [src\MyHealth.Client.Desktop] (https://github.com/Microsoft/HealthClinic.biz/tree/master/src/MyHealth.Client.Desktop). The WPF application shares code with the UWP app; the shared code is a portable library [MyHealth.Client.Core] (https://github.com/Microsoft/HealthClinic.biz/tree/master/src). 

Both WPF and the UWP get the data from the database in Azure, using the web app; the ServerUrl in file [src\MyHealth.Client.Core\AppSettings.cs] (https://github.com/Microsoft/HealthClinic.biz/blob/master/src/MyHealth.Client.Core/AppSettings.cs) needs to be set to the value of your URL. For testing, the WPF application can get the data locally.

1. To get data from the database that is created locally: run the ASP.NET application in localhost (see the instructions above for running the web site). Set the ServerUrl value in AppSettings.cs to the local host: 
```csharp
 public static string ServerlUrl = "http://localhost:34393/";
```
Leave the ASP.NET application running, then on the same machine run the WPF application: open solution 02_Demos_NativeMicrosoftApps. Set MyHealth.Client.Desktop as startup project the run the application.

2. To use the database from Azure: deploy the Web application to Azure (see instructions above for running the website) then set the ServerUrl value to your website URL.

**UWP app**

The UWP app is the patient app, which lets the user see their data: appointments, medication schedule, and allows creating new appointments. To run the UWP app: open solution 02_Demos_NativeMicrosoftApps in Visual Studio, set MyHealth.Client.W10.UWP as startup project and run. The ServerUrl string  in file [src\MyHealth.Client.Core\AppSettings.cs] (https://github.com/Microsoft/HealthClinic.biz/blob/master/src/MyHealth.Client.Core/AppSettings.cs) needs to be set to the right value, in the same way as described for the WPF app.

**iOS Xamarin app**

A Xamarin license is needed. You will be prompted to sign-in with the Xamarin account within Visual Studio when creating or loading Xamarin projects. For building the iOS app, you need a Mac with OSX Yosemite (10.10.5) or above, with XCode and Xamarin installed. See instructions for setting up your Mac at http://developer.xamarin.com/guides/ios/getting_started/. 

Visual Studio needs to have the Cross-Platform tools installed: within the Visual Studio installer, select a Custom install and check the box _Cross-Platform Mobile Development_ > _C#/.NET (Xamarin)_. 

Open solution 04_Demos_NativeXamarinApps in Visual Studio. Set MyHealth.Client.iOS as startup project. Make sure your Mac is available on the network and paired with Visual Studio. 


