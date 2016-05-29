# Exercise 4 - Application Insights


## Learnings

1. Basics about [Application Insights](https://azure.microsoft.com/en-us/documentation/articles/app-insights-overview/)
1. Connecting to Application Insights using instrumentation key
1. Analyze Application Insights data in the Azure Portal and Visual Studio


## Create Application Insights

1. **Discussion points:**
   * Discuss why telemetry and logging are important especially in Microservices architectures
   * Describe the basics of Application Insights (e.g. high-level features, architecture, pricing models, etc.)

1. Open [Azure Portal](https://portal.azure.com) and sign in.

1. Add *Application Insights* named `PracticalDevOps-Dev` to the resource group `PracticalDevOps-Dev`.<br/>
   ![Add App Insights](img/create-application-insights.png)

1. Copy *Instrumentation Key* from Azure Portal<br/>
   ![Copy Instrumentation Key](img/copy-instrumentation-key.png)


## Configure Application to Use Application Insights   

1. Add the `InstrumentationKey` setting to your `web.config` file.
   ```
    <?xml version="1.0" encoding="utf-8"?>
    ...
    <configuration>
        <appSettings>
            <add key="MinimumNumberOfBooks" value="1"/>
            <add key="MaximumNumberOfBooks" value="5"/>
            <add key="BookNameTokenUrl" value="..."/>
            <add key="InstrumentationKey" value="ba2c0764-5cfb-4741-a8e1-fb150b175a7d"/>
        </appSettings>
        ...
    </configuration>
   ```

1. Add code setting the instrumentation key to `Startup.cs`:
   ``` 
    public void Configuration(IAppBuilder app)
    {
        TelemetryConfiguration.Active.InstrumentationKey = ConfigurationManager.AppSettings["InstrumentationKey"];

        // Allow CORS
        app.UseCors(CorsOptions.AllowAll);
        ...
    }
   ```
   Either have Visual Studio add the using statement for you or you can can add the statement manually
   ```   
    using Microsoft.ApplicationInsights.Extensibility;
   ```

1. Add custom event tracking to `Controllers/BooksController.cs`:
   ```
    [HttpGet]
    [Route("books")]
    public async Task<IEnumerable<Book>> Get()
    {
        var numberOfBooks = new Random().Next(this.options.MinimumNumberOfBooks, this.options.MaximumNumberOfBooks + 1);

        var telemetryClient = new TelemetryClient();
        telemetryClient.TrackEvent($"Generating {numberOfBooks} books");

        var result = new Book[numberOfBooks];
        ...
    }
   ```
   Either have Visual Studio add the using statement for you or you can can add the statement manually
   ```   
    using Microsoft.ApplicationInsights;
   ```
   
1. See if the code compiles (it may fail if you didn't added the using statements)

1. **Discussion points:**
   * Describe basics of Application Insights SDK (e.g. exception logging, metrics)


## Run Application and View Telemetry

1. Run all your tests to make sure you did not break something.

1. Run application locally and refresh `http://localhost:2690/api/books` multiple times.<br/>
> **Note: Port number may vary**

1. Open *Search* in Application Insights (view -> other windows).<br/>
   ![Search AppInsights](img/azure-app-insights-search.png)

1. See if your application telemetry appears.

1. **Discussion points:**
   * Let people play a bit with building Application Insights dashboards in the Azure portal
   * Describe concept of custom processing of Application Insights data
   * Brief overview about other Application Insights modules (e.g. for IaaS)

1. Open *Application Insights Search* in Visual Studio while debugging your application. Refresh `http://localhost:2690/api/books` multiple times. See if your application telemetry appears.<br/>
   ![Application Insights Search](img/visual-studio-application-insights.png)

1. Open Application map in Azure Portal for the created application telemetry in this exercise. (Settings and Diagnostics -> Application Map)
What do you see?

1. **Discussion points:**
    * Point out how calls to dependent services are tracked automatically
    * Show how unnecessary calls to Blob Storage in our app become visible by analyzing Application Insights telemetry data

## Further Ideas

If you have time left, you could additionally cover topics like:

* Describe how to link Application Insights with wide-spread logging frameworks like [NLog](http://nlog-project.org/) ([Microsoft.ApplicationInsights.NLogTarget NuGet package](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/))
* Demonstrate PowerBI and Application Insights

## Further analysis

Take a look at [Application Insight Analytics](https://azure.microsoft.com/en-us/documentation/articles/app-insights-analytics/) you can use it to 
query you application insights data. 
It scales to an absurd amount of information, Microsoft uses it internally for it's cloud services. It has over analyze of 600 TB a DAY! [source](https://blogs.msdn.microsoft.com/bharry/2016/03/28/introducing-application-analytics/)  