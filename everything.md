<h1>ASP.NET MVC C# Tutorial</h1>

# Contents

- [What is NuGet](#what-is-nuget)
- [Manage NuGet Packages Dialog](#manage-nuget-packages-dialog)
- [Package Manager Console](#package-manager-console)
- [Controller Notes](#asp-net-controllers-notes)
- [Views](#views)
- [Views Pt. 2](#more-views)
- [Entity Framework](#entity-framework)
- [Dan Morain Forms](#asp-net-html-forms)
- [Forms and Helpers](#forms-and-helpers)
- [Validations](#annotations-and-validations)
- [Dropdown Lists](#how-to-create-a-dropdown-list)
- [Model within a model](#model-within-model)
- [Adding Authorization (byu, cougars)](#adding-authorization)
- [Security](#security)
- [User Identity](#user-identity)
- [Return to Previous URL](#return-to-previous-url)
- [Tutorial Explanation](#tutorial-explanation)
- [Adding a Database step-by-step instructions](#instructions-to-add-database)

## Getting Started

- Open Visual Studio
- Click on File, New Project
- Select ASP.NET MVC4 Web Application located under Templates/Visual C#/Web
- Enter `FirstMVCSite` in the name edit area

  ![mvctutorial](https://cloud.githubusercontent.com/assets/6514931/19099172/122ff62a-8a71-11e6-897a-afcd29baa9c1.png)

- Click on the OK button
- Select Empty Template and leave everything else as is

  ![2ndimage](https://cloud.githubusercontent.com/assets/6514931/19099226/9d0ccd54-8a71-11e6-9a38-4449dde0b9f5.png)

- Click on the OK button


  **NOTE:** ASP.NET will create the new framework for your web site. `RAZOR` is a view engine option that allows you to quickly integrate server code in your html through the use of blocks (uses @)

  ![solution-explorer](https://cloud.githubusercontent.com/assets/8953261/16710847/cc3d694a-45fb-11e6-93db-cdf16f4915ca.png)


  **NOTE:** Incoming requests are handled by controllers (usually inherited from System.Web.Mvc.Controller). Each public method in the controller is the action method and can be invoked from the web via a URL

- Right click the Controllers folder and choose Add, Controller
- Select MVC 5 Controller Empty and click Add

  ![mvccontroller](https://cloud.githubusercontent.com/assets/6514931/19099857/62ed9a1c-8a77-11e6-8b82-dd926ff489e5.png)

- Set the name to be `HomeController` and click Add


  **NOTE:** All controllers should have the word Controller in their name

  ![homecontroller](https://cloud.githubusercontent.com/assets/6514931/19099897/c3ebca64-8a77-11e6-8014-42a4813a5725.png)


  **NOTE:** The scaffolding options allows you to select templates with built-in common functionality. The default contents of the controller will be displayed

  ```csharp
  using System;
  using System.Collections.Generic;
  using System.Linq;
  using System.Web;
  using System.Web.Mvc;


  namespace FirstMVCSite.Controllers {
    public class HomeController : Controller {
      // GET: /Home/

      public ActionResult Index() {
        return View();
      }
    }
  }
  ```


  **NOTE:** The MVC Framework maps URLs to method which are in Controller classes

- Modify the `Index` method to look like the following:

  ```csharp
  public String Index() {
    return "Cougars beat Gonzaga for tournament title";
  }
  ```

- Run the program by clicking on the Green triangle

  ![run-btn](https://cloud.githubusercontent.com/assets/8953261/16710864/cc70ce7e-45fc-11e6-8118-d5c5b3517ec9.png)


  **NOTE:**  You can click on the down arrow to select your preferred browser

- Click on the red square to stop running the application

  ![stop-btn](https://cloud.githubusercontent.com/assets/8953261/16710869/e1e80e7a-45fc-11e6-9ea7-9d60d0266187.png)


  **NOTE:** The index action is the default method that gets executed for /, /Home, and /Home/Index meaning that if a browser requests http://yoursite/ or http://yoursite/Home the Index method will be executed


  **NOTE:** In order to generate an HTML response to a browser request, you must create a view. The Index method needs to be modified to return a ViewResult object which contains the HTML response.


- Modify the Index method to look like the following:

  ```csharp
  public class HomeController : Controller {

    // GET: /Home/

    public ViewResult Index() {
      return View();
    }
  }
  ```

- Right-click on the View Folder and choose Add, New Folder
- Name it `Home`


  ![view-folder](https://cloud.githubusercontent.com/assets/8953261/16710875/2e259302-45fd-11e6-9ad7-f5eae93a727a.png)

- Right-click on the Home Folder and choose Add, View
- Enter the name of `Index` and make sure to uncheck the Use a layout page

  ![addview](https://cloud.githubusercontent.com/assets/6514931/19100014/8e8108e8-8a78-11e6-95de-e67cd770483c.png)

- Click on the Add button


  **NOTE:** A new file will be generated called Index.cshtml (C# HTML) in the Home, View Folder

- Double-Click on the Index.cshtml file


  **NOTE:** the `@{ layout` code tells the Razor view engine that there is NOT a master page which is a common template that can be used through the entire web project


- Add the text within the `<Div>` tags

  ```html
  @{
    Layout = null;
  }

  <!DOCTYPE html>
  <html>
  <head>
    <meta name="viewport" content="width=device-width" />
    <title>Index</title>
  </head>
  <body>
    <div>
      Cougars beat Gonzaga in the tournament
    </div>
  </body>
  </html>
  ```

- Run the application


  **NOTE:** the ViewResult tells MVC to render a view and return HTML. There are different action methods we can use besides the ViewResult. For example, we could use the RedirectResult to have the browser redirect to another URL. The controller’s job is to provide input for the view who in turn renders it to HTML.


  The controller uses a ViewBag object to pass data to a view. It allows you to dynamically create “properties” for the object

- Modify the HomeController to look like the following:

  ```csharp
  public ViewResult Index() {
    String sTeamName = "Gonzaga";

    ViewBag.Opponent = sTeamName.Equals("Gonzaga") ? "Beat Gonzaga" : "Beat anyone else";

    return View();
  }
  ```

- Now modify the Index.cshtml file to look like:

  ```html
    <body>
      <div>
        @ViewBag.Opponent in the tournament
      </div>
    </body>
  ```

- Run the applcation
- Modify the HomeController file’s Index method to look like:

  ```csharp
    public ViewResult Index() {
      int hour = DateTime.Now.Hour;

      ViewBag.Greeting = hour < 12 ? "Good morning" : "Good afternoon";

      return View();
    }
  ```

- Now modify the div in the Index.cshtml file to be:

  ```html
  <div>
    @ViewBag.Greeting World
    <p>We are having a cool pool party</p>
  </div>
  ```


  **NOTE:** A model contains the processes and rules representing the real-world objects (called the domain class)

- Right-click the Models folder
- Select Add, Class

  ![add-new-item](https://cloud.githubusercontent.com/assets/8953261/16710907/d410ca38-45fe-11e6-9962-cdc7d3286b84.png)

- Enter the file name of `GuestResponse.cs` and click the Add button
- Modify the file to look like the following:

  ```csharp
  public class GuestResponse {
    public string Name { get; set; }
    public string Email { get; set; }
    public string Phone { get; set; }
    public bool? WillAttend { get; set; }
  }
  ```


  NOTE: the bool? Data type represents true, false, and null

- Modify the Index.cshtml view to be:

  ```html
  <div>
    @ViewBag.Greeting World
    <p>We are having a cool pool party!</p>
    @Html.ActionLink("RSVP Now", "RsvpForm")
  </div>
  ```


  NOTE: The Html.ActionLink request is called a helper method and is a convenient way to render HTML links, text inputs, checkboxes, and selections. It takes two parameters (text to display in the link, action to perform when the user clicks the link)

- Now we need to create the action that corresponds to RsvpForm. Add a method to the HomeController class called RsvpForm so that when it is called, it returns a ViewResult object representing the RsvpForm.

  ```csharp
  public class HomeController : Controller {

    // GET: /Home/

    public ViewResult Index() {
      int hour = DateTime.Now.Hour;

      ViewBag.Greeting = hour < 12 ? "Good morning" : "Good afternoon";

      return View();
    }


    public ViewResult RsvpForm() {
      return View();
    }
  }
  ```

- Build the project by selecting Build, Build FirstMVCSite
- Create a strongly typed view for the RsvpForm by Right-Clicking inside the RsvpForm action method and choosing Add View


  **NOTE: A strongly typed view represents a view that renders a specific type rather than a generic type. This allows you to build the HTML form for editing objects specifically tied to a model so that we can pass information from a controller to a view.**


  There are 3 ways to pass information from a controller to a view:
  1. Strongly typed model
  2. Dynamic (we haven’t talked about yet)
  3. Using a ViewBag

  ![rsvpform](https://cloud.githubusercontent.com/assets/6514931/19100322/143de63e-8a7b-11e6-82c8-ce8f1216d7fd.png)


    **NOTE: A new View will be created called RsvpForm and placed in the Views, Home folder**

    ![rsvp-form](https://cloud.githubusercontent.com/assets/8953261/16710944/697113a2-4600-11e6-9299-9f238bbb5208.png)


    **NOTE: The cshtml file contains the following Razor model line that strongly typed this view making it possible to edit the FirstMVCSite objects.**

    ```csharp
    @model FirstMVCSite.Models.GuestResponse

    @{
      Layout = null;
    }
    ```

- Modify the RsvpForm.cshtml to look like the following:

  ```html
  <body>
    @using (Html.BeginForm()) {
      <p>Name:    @Html.TextBoxFor( x => x.Name)</p>
      <p>Email:   @Html.TextBoxFor( x => x.Email)</p>
      <p>Phone:   @Html.TextBoxFor( x => x.Phone)</p>
      <p>Will you attend:    
                  @Html.DropDownListFor( x => x.WillAttend, new[]
                    {
                      new SelectListItem()
                      {
                        Text = "Yes", Value = bool.TrueString
                      },
                      new SelectListItem()
                      {
                        Text = "No", Value = bool.FalseString
                      }
                    },
                    "Please choose one")</p>
      <input type="submit" value="Submit RSVP">
    }
  </body>
  ```


  **NOTE: The TextBoxFor allows you to generate an HTML input element and works because we created a strongly typed view so that the helper methods can infer which data type we are reading from the @model expression and is similar to:**


  `<input id=”Name” name=”Name” type=”text” value=”” />`


  **The Html.BeginForm generates a form element configured to postback to the action method. Since no parameters are passed it assumes to postback to the same URL. By wrapping this statement in a C# “using” statement the helper closes the HTML form element when it goes out of scope an internally creates:**


  ```html
  <form action=”/Home/RsvpForm” method=”post”>
    …all of the content we included
  </form>
  ```

- Save the project
- Run the project


  **NOTE: We have not told MVC what to do when the form is posted so instead, it just clears the data entered into the form. The form posts back to the RsvpForm action method in the Home controller which says to render the view through the statement “return View()”**

- In the HomeController, add the following to tell MVC which action method in the RsvpForm represents the HTTP GET requests

  ```csharp
  public class HomeController : Controller {

    // GET: /Home/

    public ViewResult Index() {
      int hour = DateTime.Now.Hour;

      ViewBag.Greeting = hour < 12 ? "Good morning" : "Good afternoon";

      return View();
    }

    [HttpGet]
    public ViewResult RsvpForm() {
      return View();
    }
  }
  ```


  **NOTE: Adding the [HttpGet] tells MVC that the method will be responsible for displaying the initial blank form when someone first visits the RsvpForm and the method is only used for GET requests**

- Now add an import to the FirstMVCSite Models so that you can reference the model without having to qualify the class name

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;
    using FirstMVCSite.Models;


    public class HomeController : Controller {

      // GET: /Home/

      public ViewResult Index() {
        int hour = DateTime.Now.Hour;

        ViewBag.Greeting = hour < 12 ? "Good morning" : "Good afternoon";

        return View();
      }

      [HttpGet]
      public ViewResult RsvpForm() {
        return View();
      }

      [HttpPost]
      public ViewResult RsvpForm(GuestResponse response) {
        return View("ThankYou", response);
      }
    }
  ```


  **NOTE: This tells MVC that the overloaded RsvpForm method receives a single parameter of type GuestResponse which is in our FirstMVCSite model. It then returns a View object (ThankYou) and GuestResponse object (response) which is passed to the View and is used for Post requests.**

- Create the new view ThankYou by Right-Clicking inside the overloaded HomeController ViewResult method and choosing Add View




  ![thankyouview](https://cloud.githubusercontent.com/assets/6514931/19100421/f80e171c-8a7b-11e6-91ef-947ed44a6dc6.png)


  **NOTE: The ThankYou.cshtml file for the view will be created in the Home Controllers folder**

  ![thank-you-file](https://cloud.githubusercontent.com/assets/8953261/16711030/7b68752e-4604-11e6-8064-a0ea0ef84198.png)

- Modify the ThankYou.cshtml file to be as follows:

  ```html
  <body>
    <div>
      <h1>Thank you @Model.Name</h1>
      @if (Model.WillAttend == true)
      {
        @:We look forward to seeing you
      }
      else
      {
        @:Hopefully you can come next time
      }
    </div>
  </body>
  ```


  **NOTE: Using the Model we can bind data to keys and values we can now interface with the C# objects rather than trying to deal with the Request.Form[] and Request.QueryString[] elements. The GuestResponse object is automatically populated with the data from the form fields.**

- Save and run the project


  **NOTE: ASP.NET MVC supports validation rules through the use of the System.ComponentModel.DataAnnotations namespace**  

- Open the GuestResponse.cs Model
- Enter the following information for each of the get/sets:


  **NOTE: MVC automatically detects the attributes within the [ ] and uses them to validate the data in the model-binding process.**


  Also, take note of the bool? Value. It represents a 3 state Boolean of null, true, and false.


  Remember, the Model defines the rules but the Controller enforces them.

  ```csharp
  public class GuestResponse {
    [Required(ErrorMessage = "Please enter a name")]
    public string Name { get; set; }

    [Required(ErrorMessage = "Please enter an email")]
    public string Email { get; set; }

    [Required(ErrorMessage = "Please enter a phone")]
    public string Phone { get; set; }

    [Required(ErrorMessage = "Please indicate if you will attend")]
    public bool? WillAttend { get; set; }
  }
  ```
- You will also need to add 'using System.ComponentModel.DataAnnotations;' to this file in the Using area

- Open the HomeController.cs Controller
- Add the following code to the HttpPost attribute

  ```csharp
  [HttpPost]
  public ViewResult RsvpForm(GuestResponse response) {
    if(ModelState.IsValid) {
      return View("ThankYou", response);
    } else {
      return View();
    }
  }
  ```


  **NOTE: This code tells MVC that if the validations passed then display the View using the ThankYou.cshtml view object and GuestResponse object response. Otherwise, just render the RsvpForm view without any parameters**

- Now we need to provide some way of telling the user there was a problem if a validation rule was broken. Open the RsvpForm.cshtml View
- Add the following code:

  ```html
  @using (Html.BeginForm()) {

    @Html.ValidationSummary();

    <p>Name:    @Html.TextBoxFor( x => x.Name)</p>
    <p>Email:   @Html.TextBoxFor( x => x.Email)</p>
    <p>Phone:   @Html.TextBoxFor( x => x.Phone)</p>
    <p>Will you attend:    
                @Html.DropDownListFor( x => x.WillAttend, new[]
                  {
                    new SelectListItem()
                    {
                      Text = "Yes", Value = bool.TrueString
                    },
                    new SelectListItem()
                    {
                      Text = "No", Value = bool.FalseString
                    }
                  },
                  "Please choose one")</p>
    <input type="submit" value="Submit RSVP">
  }
  ```


  **NOTE: The Html.ValidationSummary method creates a holding list of items that failed validation. This list is hidden on the form so the end user does not see it. MVC add the error messages to this list for every broken validation rule. **


  **Notice that the data entered into the form is not cleared. This is a benefit of model binding.**

- Save and run the program


  **NOTE: HTML Helper methods simply return a string and are used insert HTML tag within your code. For example, the Html.BeginForm() Helper method is used to create the opening and closing HTML <form> tags.  The Html.TextBox() Helper method is used to create input tags.**


  Previously in the RsvpForm.cshtml View, we placed the code:
  `@Html.TextBoxFor( x => x.Name)`


  This Html helper method generates code that looks like:
  `<input data-val=”true” data-val-required=”Please enter your name” id=”Name” name=”Name” type=”text” value=”” />`


  When a validation rule is broken for this tag, the following code automatically is generated:
  `<input class=”input-validation-error” data-val=”true” data-val-required=”Please enter your name” id=”Name” name=”Name” type=”text” value=”” />`


  The HTML Helper method added a class to the tag called input-validation-error.


  We can add a CSS style for this class and others that HTML Helper methods invoke

- Right-click on the FirstMVCSite and select Add, New Folder
- Type in the name `Content`

  ![content](https://cloud.githubusercontent.com/assets/8953261/16711102/b6464e3e-4607-11e6-92c0-cf0942d02ee4.png)

- Right-click on the Content folder and choose Add, New Item
- Scroll down and select Style Sheet and click on the Add button

  ![style-sheet](https://cloud.githubusercontent.com/assets/8953261/16711109/e78c0e84-4607-11e6-8c1b-bfe59c32ab11.png)

- Right-click on the newly added Style Sheet and choose Rename
- Change the name to Site.css

  ![site-css](https://cloud.githubusercontent.com/assets/8953261/16711118/18dccafa-4608-11e6-9243-12f1f950ec5c.png)

- Modify the Site.css to be the following (NOTE: Remove the .body tag)

  ```css
  .field-validation-error {
    color: #f00;
  }
  .field-validation-valid {
    display: none;
  }
  .input-validation-error {
    border: 1px solid #f00;
    background-color: #fee;
  }
  .validation-summary-errors {
    font-weight: bold;
    color: #f00;
  }
  .validation-summary-valid {
    display: none;
  }
  ```

- Open the RsvpForm.cshtml View
- Add the following code:

  ```html
   <head>
    <meta name="viewport" content="width=device-width" />
    <link rel="stylesheet" type="text/css" href="~/Content/Site.css" />
    <title>RsvpForm</title>
  </head>
  ```

- Save and run the project



# NuGet Package Manager

## Contents

- [What is NuGet](#what-is-nuget)
- [Manage NuGet Packages Dialog](#manage-nuget-packages-dialog)
- [Package Manager Console](#package-manager-console)


### What is NuGet?

[NuGet](https://github.com/nuget/home) is the package manager (i.e. libraries) for Microsofts .NET development platform. It serves as a repository for developers to upload and download software that can be used within their .NET applications. As of August 18, 2015, there are almost 41,000 packages that can be downloaded and installed to save you time and effort in getting you job done.

When you add a package to your .NET environment, its dependencies are also installed. When a package is updated, its dependencies are also updated. NuGet helps with many small management tasks like these.

There are 2 ways to access NuGet within the .Net environment: the `Manage NuGet Packages` dialog and the package manager console.

Although not included in this course, it is good to know that _you_ have the ability to create your own packages to share software with other developers. If you're interested in learning more about participating in the community, go to the [NuGet GitHub repository](https://github.com/nuget/home) or check out some open source NuGet packages like the [MongoDB C# Driver](https://github.com/mongodb/mongo-csharp-driver) or the [PayPal Merchant SDK](https://github.com/paypal/merchant-sdk-dotnet).


### Manage NuGet Packages Dialog

The `Manage NuGet Packages` dialog is accessed by right-clicking the References node in the solution explorer and choosing `Manage NuGet Packages`. This dialog window allows you to see all packages currently installed and search for software available to download and incorporate into your application. The `Manage NuGet Packages` dialog also helps you keep packages updated.

The settings button allows you to configure how you want NuGet to run; i.e. automatically check for and download missing packages. As you click on each available package, NuGet will display information such as package dependencies, version, description, and more.

[http://nugetmusthaves.com/Category/MVC](NuGetMustHaves.com) shows the "must haves" for MVC that you can download and install.


### Package Manager Console

The Package Manager Console provides a shell based console window to install and maintain packages and provides a little more control over the NuGet options. You can run the console by navigating to `Tools > Library Package Manager > Package Manager Console` _(NOTE: the console window will be displayed)_. In the console window you have the ability to run actions (i.e. get-package, install-package, etc.).

If you would like to use the console rather than the dialog you can review the commands in the [NuGet package manager console documentation](https://docs.nuget.org/consume/package-manager-console) or on their [GitHub](https://github.com/nuget/home)




### ASP NET Controllers Notes
- MVC is a framework used in Visual Studio that allows you to write web apps
- A framework is simply a library of code that we call to make development easier
- An MVC Visual Studio solution contains your project(s) to make your app
- M is for model which represents data (C# code)
- V is for View which represents what the user sees (HTML)
- C is for Controller which is the middle person between the model and the view. It gets data from the model and passes it to the view (html) and gets data from the view and passes it to the model (table) (C# code)
- Razor is another language we use in .net that allows us to insert or embed source code inside of html
- If you see the @ sign then you know it is a razor command
- Viewbag is an asp.net object used in the project and is created dynamically. You can create attributes dynamically in the viewbag and store anything you want in it.
- A viewbag can have more than one attribute
- You access the viewbag using razor @ViewBag.attribute
- The routeconfig file tells the system to start in the Home Controller and the Index method in that controller
- A controller method will automatically look for an html page in the view folder with the same name by default
- The controller (HomeController) inherits from the Controller class
- The methods in the controller class you made are just C# methods and are usually the ones that will gather data and pass it to the view
- The controller method interacts with the view by returning a view object (ViewResult) which by default, ASP will look at the method name (i.e Index) and when you say return View() it finds a matching view in the same name folder as where the controller is being stored (Home)
- A controller is a public method that acts upon something (we call them actions)
- A controller responds to users URL requests and then performs appropriate actions for that request
- A controller also returns the response back to the browser that invoke the URL request
- All URL requests go through a controller
- The routeconfig in the MVC framework maps the URLs to a method, not a web page. Those methods are found in the controller classes
- As the tutorial shows, the methods in a controller can be an [HttpGet] or an [HttpPost]. The [HttpGet] attribute on a controller is responsible for displaying a blank input form when the user first visits the URL. The [HttpPost] attribute on a controller is used for storing data back from a form.
- One method in a controller can be BOTH a [HttpGet] AND a [HttpPost]


# Views


## Purpose

- Responsible for providing the UI
- Views are not directly accessible like other frameworks (i.e. web forms, PHP, etc.)
- You cannot point your browser to a view and render it
- A view is rendered by a controller
- The controller can pass a model to the view order to provide data. The view then transforms the model into a format ready to be presented to the user (HTML)
- Remember that the controller just sends the necessary data back to the view which then renders it to HTML. IOW, you could return `"Hello " + "<strong>Greg</strong>"` and it would Bold the word **Greg**


## ViewBag

- Great for passing little bits of data but DON'T rely upon it for everything!
- ViewBag is a property of ControllerBase which all controllers inherit from up the line
- It is a dynamic object
- When you call return View() from the controller it creates a new instance of ActionResult passing ViewData from the controller to it's constructor which allows it to be used and accessed in the view.
- Each view will end up with its own ViewBag since it is not static (that is on purpose so that you don't have two concurrent users accessing the same ViewBag
- Its lifetime is the same as the controller which is the lifetime of the HTTP request
- The best times to use it are:
  - Incorporating dropdown lists of lookup data into an entity
  - Components like a shopping cart
  - Small amounts of aggregate data
- The ViewBaginfo is passed from the controller to the view via a ViewDataDictionary (think of your data structures)


## How do views work?

- Each controller contains a view file for each action method
- The view file is named the same as the action method
- Assume that you had a controller called HomeController
- You would also want a View folder called Home
- ASP.NET MVC knows that if you have a controller called HomeController then it should be linked to a view in the Home folder
  `public ActionResult Index()` – means that there should be a view with that name if you do the return View()
0 IOW, the view selection logic looks for a view with the same name as the action with the ControllerName directory (without the controller suffix)
- When you type in a URL you are not specifying a view. Instead you are specifying a controller.
  - www.flaxom.com/Test/Index would look for a controller called IndexController and then map to the View folder called Test looking for an Action called Index. In this action (method) you could return View() which means that it would then look for a view called Index.cshmtl in the Test View folder. However, you do NOT have to return View() but could instead return View("Myfile"); which would still look in the Test View folder. You could even specify a view that is NOT in the Test view folder: return View("~Views/NewTest/Index.cshtml");
- Views are attached to an action (method in the controller)


## Routes

- ASP.NET Routing system decides how URLs map to controllers and actions
- Default routes are added when a project is created to get you started
- Look in the RouteConfig.cs file for the routes


### Adding a Controller

- Create a new, Empty, MVC project (check box) and click OK
- Right click on Controllers, Add
- Empty Controller named Home
- Run and look at the errors
- It shows the search path, the View for Index was not found in the Home directory
- Right mouse click within the controller action method and choose Add View
- Uncheck Partial and Layout and click Add
- You can return a view or even redirect to another page (known as Action Result Objects)

```csharp
public ActionResult Index() {
  return new RedirectResult("http://www.ksl.com");
}
```

- Add the following

```csharp
// GET: /HOME/

public ActionResult Index() {
  int hour = DateTime.Now.Hour;

  ViewBag.Greeting = hour < 12 ? "Good Morning" " : "Good Afternoon";

  return View();
}

```

### Modify View

- Modify the Index view and add the following code:

  ```html
  <body>
    <div>
      @ViewBag.Greeting World

      <p>Having a party!!!</p>

      @Html.ActionList("RSVP Now", "RsvpForm")
    </div>
  </body>
  ```

- `Html.ActionLink` is a helper method
- They include methods for:
  - links
  - text inputs
  - checkboxes
  - selections
  - and more
- ActionLink takes 2 parameters:
  1. Text to display in the link
  2. Action method to perform when the text is clicked

- Run and test the program


  NOTE: if you click the link it will try to call the RsvpForm action method which does not exist

### Write another Controller

- Go to the Controllers folder and modify the HomeController file
- Write the following action method:

  ```csharp
  public ViewResult RsvpForm() {
    return View();
  }
  ```

- You can use either the ViewResult or the ActionResult return type

### Strongly Typed View

- A strongly typed view renders a specific type
- Make sure you have compile your project first in case you added your model but have never compiled
- The system will need to know about the model for a strongly typed view since it is tied TO the view

### Creating a Model

- In order to make a view a Strongly Typed View we need to work with a model
- Right mouse click on the Models folder and choose Add|Class
- Name the class GuestResponse.cs and click on the Add button
- A model is simply a C# class that mimics the structure of the data you want to save


### GuestResponse Model

- Type in the following code in the newly created model:

```csharp
public class GuestResponse {
    public string Name { get; set; }
    public string Email { get; set; }
    public string Phone { get; set; }
    public bool? WillAttend { get; set;}
}
```

- The ? On the bool allows the system to store a null, true, and a false
- TRICK: If you type prop and press tab twice it will generate a lot of the code for you
- Save the project and then BUILD (you have to build to let the system know about the model and be able to integrate it within the controllers and views)
- TRICK: In the C# Source code editors can you start to type a letter and a list will be displayed. Some of these items will autogenerate source code if you press tab twice (i.e. p tab tab, prop tab tab, mvcaction4 tab tab, etc.) – found in Tools | Code Snippets Manager


### RsvpForm View

- With the model now in place, go back to the Home controller and Right click in the RsvpForm action method and choose Add View
- Set template to Empty
- Choose the GuestResponse model and click on the Add button
- Notice the model included in the view (Change the NameOfProject to match your project name)
- @model NameOfProject.Models.GuestResponse
- NOTE: You can change to a strongly typed view by including the model in the view


### Adding Bootstrap and JQuery using NuGet Package Manager

- You can add bootstrap and jquery to an empty view/project by using NuGet
- Click on Tools | NuGet Package Manager | Manage NuGet Packages for Solution
- If it is not already installed in this project, click on Online and Search for Bootstrap (upper right corner)
- Click on Bootstrap CSS and click on Install
- You can also install Jquery by clicking on it and clicking Install
- The Content folder will automatically be created and the necessary CSS files will be placed in it along with the necessary script files in the Scripts folder


### HTML Head

- Make sure you add the necessary tags for Bootstrap and Jquery to work within your project
- Just because added the necessary references and files to the project it does not automatically incorporate it within your view
- Add the following code to the RsvpForm View:

```html
<head>
    <meta name="viewport" content="width=device-width" />
    <title>RsvpForm</title>
    <link href="~/Content/bootstrap.min.css" rel="stylesheet" />
    <script src="~/Scripts/jquery-2.1.4.min.js"></script>
    <script src="~/Scripts/bootstrap.min.js"></script>
</head>
<body>
  <div class="jumbotron">
    <div class="container">
      @using (Html.BeginForm())
      {
        <p>Name: @Html.TextBoxFor(x => x.Name)</p>
        <p>Email: @Html.TextBoxFor(x => x.Email)</p>
        <p>Phone: @Html.TextBoxFor(x => x.Phone)</p>
        <p>
          Will you attend? @Html.DropDownListFor(x => x.WillAttend, new[] {
                                      new SelectListItem()
                                      {Text = "Yes, I will be there", Value = bool.TrueString},
                                      new SelectListItem()
                                      {Text = "Nope", Value = bool.FalseString} }, "Choose an option")
        </p>
        <input type="submit" value="Submit RSVP" />
      }
    </div>
  </div>
</body>
```

### HTML Helpers Explained

`@using (Html.BeginForm())`

- The Razor using statement allows you to specify an HTML helper. In this case you are having Razor generate the necessary HTML code for a form to get input
- The @Html.TextBoxFor helper is used when you want to associate a property of a model to the textbox.  The text box then takes on the name of the property, which makes it convenient to gather all form elements as properties of a model in your post-action.


  `<p>Name: @Html.TextBoxFor(x => x.Name)</p>`
- The @Html.DropDownList helper is used to create a drop down list. However, you are responsible for creating the items in the list by calling the new SelectListItem() and passing it the text and value


  ```csharp
  @Html.DropDownListFor(x => x.WillAttend, new[] {
                                                  new SelectListItem()
                                                  {Text = "Yes, I will be there", Value = bool.TrueString},
                                                  new SelectListItem()
                                                  {Text = "Nope", Value = bool.FalseString} }, "Choose an option")
  ```


### Lambda Expression

- A lambda expression is away to write an anonymous function, i.e. a function without a name. What you have on the left side of the "arrow" are the function parameters, and what you have on the right side are the function body. Thus, (x => x.Name) logically translates to something like string Function(Data x) { return x.Name } the types string and Data will obviously vary and be derived from the context.
- The model => item.FirstName means logically it would translate to string

  ```csharp
  Function(Model model) {
    return item.FirstName;
  }
  ```
- Basically this is a function with a parameter, but this parameter is not used.


### HTML Forms

- The Html Helper BeginForm indicates the beginning of an html form that will be used to gather input
- An EndForm is not needed since the BeginForm helper automatically closes out the form element
- The `<input>` element is the most important form element
- `<input type="submit">` defines a button for submitting a form to a form-handler


  `<input type="submit" value="Submit RSVP" />`
- This states that there will be a button with the text Submit RSVP and will be used to submit the form to the server once clicked


### Controllers for HttpGet and HttpPost

- The HttpGet request attribute is what the browser issues each time someone clicks on a link and will be responsible for display the initial blank form
- The HttpPost request attribute is responsible for receiving submitted data and deciding what to do with it
- Both of the action methods will have the same name but MVC will make sure the correct action method is selected based upon whether the system is getting the data or the user clicked on the submit button
- In order to use the model in the controller you need to make sure it is imported


  `using EmptyView.Models;` (where EmptyView is the name of the project)

### HttpGet

```csharp
[HttpGet]
public ViewResult RsvpForm() {
  return View();
}
```

- This action method is used to render the view RsvpForm.cshtml and generate the form
- The square brackets indicate it is an attribute and the HttpGet is a reserved word to MVC
- NOTE: Make sure you have added the proper using statement to make sure you have access to the model in your controller

  ```csharp
  using WebApplication4.Models; //where WebApplication4 is the name of your project
  ```

### HttpPost

```csharp
[HttpPost]
public ViewResult RsvpForm(GuestResponse guestResponse) {
  return View("Thanks", guestResponse);
}
```

- The post action method is using Model Binding where the incoming data (guestResponse of type GuestResponse) is parsed and the key/value pairs in the Http request are used to populate the properties of the domain model type
- The names of the input elements are used to set the values of the properties in the instance of the model class which is then passed to the Post-enabled action method (the form fields values are automatically passed to the guestResponse object model properties)


### Modify the HomeController

```csharp
// GET: /Home/

public ActionResult Index()
{
  int hour = DateTime.Now.Hour;
  ViewBag.Greeting = hour < 12 ? "Good Morning" : "Good Afternoon";
  return View();
}

[HttpGet]
public ViewResult RsvpForm()
{
  return View();
}

[HttpPost]
public ViewResult RsvpForm(GuestResponse guestResponse)
{
  return View("Thanks", guestResponse);
}
```


**NOTE: Don't forget to add the using for the models" using EmptyView.Models;“ where EmptyView is the name of the project**


### Thank You View using the Model

- Let's create another view that uses the model that was populated by the RsvpForm view
- Right mouse click in any of the HomeController methods and choose Add View
- Call the view Thanks and choose the Empty Template
- Then choose the GuestResponse model


**NOTE: Visual Studio will create the Thanks.cshtml view. Notice the @model statement?**


- Write the following html for the Thanks View

  ```html
  <body>
    <div>
      <h1>Thank you @Model.Name</h1>
      @if (Model.WillAttend == true)
      {
          <p>We look forward to seeing you</p>
      }
      else
      {
          <p>Hopefully next time you can make it</p>
      }
    </div>
  </body>
  ```

### Validation in the model

- ASP.Net MVC supports declarative validation rules define with attirbutes
- Let's modify the model to require input using the [Required] validation attribute

  ```csharp
  public class GuestResponse {
    [Required(ErrorMessage = "Please enter your name")]
    public string Name { get; set; }

    [Required(ErrorMessage = "Please enter your email")]
    public string Email { get; set; }

    [Required(ErrorMessage = "Please enter your phone number")]
    public string Phone { get; set; }

    [Required(ErrorMessage = "Please specify if you will attend")]
    public bool? WillAttend { get; set; }
  }
  ```


  NOTE: Notice that the Required usage generates an error? That is because you need to use the data annotations class. Right mouse click on one of the errors and choose Resolve | using System.ComponentModel.DataAnnotations;


  If the WillAttend is null then it will generate an error message


### Validation in the Controller

- Now that the model is set up, let's add the code to the controller
- The ModelState.IsValid property checks to see if all items pass validation
- You will add this C# statement in your controller and if the model does not validate then you will return the view for the user to fix the problems

  ```csharp
   [HttpPost]
    public ViewResult RsvpForm(GuestResponse guestResponse) {
      if (ModelState.IsValid) {
        return View("Thanks", guestResponse);
      } else {
        //Validation Error
        return View();
      }
    }
  ```


###  More Views

  -	An action method (just a method in c# in a controller) returns an object from the View method which is then outputted to a browser
  -	In order to get a view, you cannot use the name of the HTML file in the Browser URL. Instead, you use a route which indicates the controller and the action method. This action method then returns the View object to the browser
  -	If the name of the method is Index and you return View() then .NET will try to find a view with the name of Index.cshmtl located in the same director structure as where the controller is stored. IOW, if the controller was stored in the Home folder then .NET would try to find the Index.cshtml file in the Views Home folder
  -	We like to use the ViewBag to pass data to a view. There are other ways to do so (ViewData) but is easier to use. For example, the following are the same concept (NOTE: the property names are case sensitive):
  ViewBag.Stuff = “Greg”;
  ViewData[“Stuff”] = “Greg”;
  -	When working with Views, you can implement scaffolding. Scaffolding allows you to generate views based upon pre built templates. It allows you to generate a view quickly based upon certain functionality that you want it to execute that has already been made by the system. IOW, you tell the system to create a view and choose the type of view you want to create (Emptpy, Read/Write, Entity Framework, etc)
  -	Razor is a View engine that allows us to embed code inside of HTML. It is very clean code, easy to read, has a small overhead (lightweight), and very powerful.
  -	In order to use Razor code blocks (lines of code) you need to use the @ sign and {} with the code between the brackets.
  -	If you want to call a razor command you simply use @code
  -	Other view engines (but we will only use razor) are ASPX (uses % signs), Spark, and NHaml
  -	If you want to have access to a model in a view you execute the @model razor command and specify the model name. For example, the following says to use the Movie model in the Models folder within the MVCMovie App:
  @model MVCMovie.Models.Movie
  -	The view contains html and is rendered and sent to the browser and is what the user sees
  -	You can make a view strongly typed meaning that you can link it to a model. Then you have access to the model structure and you can use @HTML helpers to work with the model data and model structure
  -	The MVC (model, view, controller) really means that you are working with the data layer, presentation layer, and the business layer. The data layer (model) is the database table or data structure. The presentation layer (view) is what the user sees. The business layer (controller) manages the data and the html.
  -	Developers can incorporate scaffolding when creating views in order to save time, reduce the number of errors that can be made, and avoid rewriting a lot of code. It lets the user create views quickly based upon a template
  -	@HTML Helpers are razor’s way of helping you to generate html code by implementing syntax that is easier to create and maintain
  -	Some of the HTML Helpers are http://www.tutorialsteacher.com/mvc/html-helpers


  # Entity Framework


  ## Contents

  1. .NET Entity Framework (EF)
  2. Models
  3. Navigational Properties
  4. Creating a Model
  5. Foreign Keys
  6. Creating your own Primary Key Value
  7. DbContext
  8. Working with the Database
  9. Seeding the Database
  11. LocalDB
  12. Controllers and Data


  ## .NET Entity Framework (EF)

  - NET Framework data-access technology is known as the Entity Framework (Also known as EF)
  - Supports a development paradigm called Code First
  - Code First is when you create model objects by writing simple classes
  - Now you can create the database from the model by the system
  - Creating models is like creating the skeleton of a database table


  ## Models

  - You create models in the Models folder

    ```csharp
    public class Student
    {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstMidName { get; set; }
      public DateTime EnrollmentDate { get; set; }

      public virtual ICollection<Enrollment> Enrollments { get; set; }
    }
    ```

  - The ID property becomes the primary key for the column and can either be named ID or classnameID
  - This happens by default by the EF and you do not control it


  ### Navigational Properties

  - Enrollments is a navigational property meaning. that they hold an entity relating to other entities (similar to a foreign key)
  - For example, the Enrollments property of this Student entity will hold all of the Enrollment entities that are related to that Student entity
  - If a student row (entity) was enrolled in two courses, then the Enrollments property would hold the two enrollment rows
  - Navigational properties are defined as virtual so that "lazy loading" can occur
  - Lazy loading means that when the entity is first read, related data isn't retrieved. But the first time you try to access the navigation property, the data required for that navigation property is automatically retrieved. This results in multiple queries sent to the database — one for the entity itself and one each time that related data for the entity must be retrieved.
  - If the navigational property can hold more than one entity (more than one enrollment row) then it must be defined as an ICollection data type
  - This is automatically handled by the DbContext class


  ### Creating a Model

  - You can create a new model by adding a new class to the Models folder
  - .NET looks for a property that is either ID or classnameID and sets it as the primary key (NOTE: if you use ID without classname it makes it easier to implement inheritance in the data model.)
  - For example, in the code below, EnrollmentID is automatically declared as the primary key

  ```csharp
  public class Enrollment
  {
    public int EnrollmentID { get; set; }
    public int CourseID { get; set; }
    public int StudentID { get; set; }
    public Grade? Grade { get; set; }

    public virtual Course Course { get; set; }
    public virtual Student Student { get; set; }
  }
  ```


  #### Foreign Keys

  - If you notice, CourseID is of type int. That means it is a foreign key to the Course class but only holds one entity of type Course
  - The same goes for the StudentID navigational property
  - The Grade property is of type Grade. This "could" be a class. However, if we added the following to the model then we see it is an enum. Meaning that it can be either the value A, B, C, D, or F.

  ```csharp
  public enum Grade
  {
    A, B, C, D, F
  }
  ```

  - The question mark ? tells the system that the value can be null (unknown or not assigned yet)
  - The syntax for  a foreign key is <navigation property name><primary key property name> or <primary key property name>  (CourseID is the primary key in the Course model so you could just use that same primary key as a foreign key in another model)


  #### Creating your own Primary Key value

  ```csharp
  public class Course
  {
    [DatabaseGenerated(DatabaseGeneratedOption.None)]
    public int CourseID { get; set; }
    public string Title { get; set; }
    public int Credits { get; set; }

    public virtual ICollection<Enrollment> Enrollments { get; set; }
  }
  ```

  - The DatabaseGenerated(DatabaseGeneratedOption.None) indicates that the source code will be responsible for creating the primary key value rather than the EF
  - CourseID is the primary key
  - Enrollments is navigational property and there can be more than one enrollment record associated with each course record


  ## DbContext

  - The database context class (DbContext) is the foundation class used to create an entity included in the model
  - When you create a new model it is based upon the System.Data.Entity.DbContext class
  - You will want to create a model for each table you want to work with from the database
  - The DbContext class acts as the database
  - Within the DbContext class you define DbSets (representing the tables or entitites)
  - By default, the EF tries to pluralize the variable names you created using the DbSet class

  ```csharp
  public class SchoolContext : DbContext
  {
    public SchoolContext() : base("SchoolContext") {}

    public DbSet<Student> Students { get; set; }
    public DbSet<Enrollment> Enrollments { get; set; }
    public DbSet<Course> Courses { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
      modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
  }
  ```
  - As stated earlier, the DbSet class represents an entity (table) within the database
  - If you have a navigational property you do not really need to create a DbSet for it since the EF does it for you. However, it is probably more readable if you do. In this case you did NOT have to create it for Enrollment or Course since Student refers to those entities


    `public SchoolContext() : base("SchoolContext")`

  - This is the name of the connection string that will be added to the Web.config file.
  - If you don't specifically specify the connection string then the EF assumes it is the name of the class (in this case it is SchoolContext – same name as what you specified – BUT more readable)


    `modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();`

  - This statement in the OnModelCreating method prevents the default table names from being pluralized (student class becomes students class – big pain!) – Your choice to include the statement


  ## Working with the Database

  - Default behavior is to create a database only if it doesn't exist (and throw an exception if the model has changed and the database already exists)
  - You can specify a seed method to recreate the database and load it with info


    `public class SchoolInitializer : System.Data.Entity. DropCreateDatabaseIfModelChanges<SchoolContext>`

  - This method specifies that it will be called if the model changes. It will then drop the database and recreate it
  - Then you can include a seed method in this class that loads the data into the DbContext and saves it


  ### Seeding the Database

  ```csharp
  protected override void Seed(SchoolContext context)
  {
    var students = new List<Student> {
      new Student{FirstMidName="Carson",LastName="Alexander",EnrollmentDate=DateTime.Parse("2005-09-01")},
      new Student{FirstMidName="Meredith",LastName="Alonso",EnrollmentDate=DateTime.Parse("2002-09-01")},
      new Student{FirstMidName="Arturo",LastName="Anand",EnrollmentDate=DateTime.Parse("2003-09-01")},
      new Student{FirstMidName="Gytis",LastName="Barzdukas",EnrollmentDate=DateTime.Parse("2002-09-01")},
      new Student{FirstMidName="Yan",LastName="Li",EnrollmentDate=DateTime.Parse("2002-09-01")},
      new Student{FirstMidName="Peggy",LastName="Justice",EnrollmentDate=DateTime.Parse("2001-09-01")},
      new Student{FirstMidName="Laura",LastName="Norman",EnrollmentDate=DateTime.Parse("2003-09-01")},
      new Student{FirstMidName="Nino",LastName="Olivetto",EnrollmentDate=DateTime.Parse("2005-09-01")}
    };

    students.ForEach(s => context.Students.Add(s));
    context.SaveChanges();
  }
  ```

  - You can call this seed method by modifying the Web.config file

  ```xml
  <entityFramework>
    <contexts>
      <context type="ContosoUniversity.DAL.SchoolContext, ContosoUniversity">
        <databaseInitializer type="ContosoUniversity.DAL.SchoolInitializer, ContosoUniversity" />
      </context>
    </contexts>
  </entityFramework>
  ```

  - The context type specifies the fully qualified context class name and the assembly it's in, and the databaseinitializer type specifies the fully qualified name of the initializer class and the assembly it's in
  - When you don't want EF to use the initializer, you can set an attribute on the context element: disableDatabaseInitialization="true"
  - When you access the database for the first time in running the application the Entity Framework compares the database to the model. If there's a difference, the application drops and re-creates the database.


  ## Local DB

  - LocalDB is a lightweight version of the SQL Server Express Database Engine and comes with the EF and .NET (not recommended for production though since it does not work with IIS)
  - You can put LocalDB database files in the App_Data folder of a web project
  - To work with a LocalDB SQL database you must add a connection string to the Web.config file in the root folder

  ```xml
  <connectionStrings>
    <add name="SchoolContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;Initial Catalog=ContosoUniversity;Integrated Security=SSPI;" providerName="System.Data.SqlClient"/>
  </connectionStrings>
  ```

  - The DbContext class "SchoolContext" handles the task of connecting to the database and mapping objects to database records. The connection string in the Web.config file specifies the name and type of database.


  ## Controllers and Data

  - You can create a controller that works with the EF and views
  - When creating the controller you can select a model already created which creates a link between the view and the data structure
  - You can also specify the Data Context which specifies how the objects will be mapped to the data
  - Since the model is in place along with the Data Context and Connection String, the controller has everything it needs to create the code necessary to work with the data



  # ASP NET HTML Forms

  This tutorial about HTML forms will take less than 30 minutes

  ## Introduction

  **This is what a form looks like in HTML**  



  ```html
  <form action="/Home/Sign_in" method="POST">
  	<input name="email" type="email" />
  	<input name="password" type="password" />
  	<input type="submit" value="Sign in" />
  </form>
  ```



  Form Component | Description
  -------------- | -----------
  Action         | Tells the browser where to send the information the user enters into the form
  Method         | GET or POST
  Input          | Create form compontents (text box, button)
  GET            | Requests data from a specified resource
  POST           | Submits data to be processed to a specified resource



  **We build forms in .NET using the Html form helper class**



  ```
  @using (Html.BeginForm("Sign_in", "Home", FormMethod.Post))
  {
      @Html.TextBox("Email address")
      @Html.Password("Password")
      <button type="submit">Sign in</button>
  }
  ```
  We are building a view that displays a sign in form to the user and submits the form data to the Sign_in action in the Home Controller

  ![picture of complete product](images/finished.png)

  ## Getting Started

  ### Download Starter Code

  - You can download this project by clicking the download "Download ZIP" button on the right side of this page
  - Run the project and make sure it displays an empty view for us to build our form in

  ![inital load screen when running the project](images/initial-run.png)

  ### View: Build form with HTML Helper

  Add this code to your index.cshtml file

  ```
  @using (Html.BeginForm("Sign_in", "Home", FormMethod.Post))
  {
      <label for="inputEmail">Email address</label>
      @Html.TextBox("Email address")
      <label for="inputPassword">Password</label>
      @Html.Password("Password")
      <button type="submit">Sign in</button>
  }
  ```

  ![build form in index.cshtml](images/build-form.png)

  Your view should now show an ugly form for the user to input an email address and password

  ![build form in index.cshtml](images/ugly-form.png)

  Before this form will work we need to create an action method in our Home Controller to receive this request

  ### Controller: Create Sign_in Action method

  **Add a method to your Home Controller called Sign_in.**  We use the `[HttpPost]` annotation to specify that this method is receiving data

  ``` csharp
  [HttpPost]
  public String Sign_in(FormCollection form)
  {
      String email = form["Email address"].ToString();
      return "Signing in : " + email;
  }
  ```

  ![add code to controller](images/controller.png)

  Submitting the form passes the form data to the Sign_in action.  We retreived the email address from the form and returned a string to give feedback to the user.

  ![submitting form](images/submitting-form.png)

  ### Make it pretty: Add html attributes to form

  You can add html attributes to the form by passing an object to the Html helper's constructor

  ```
  Html.BeginForm("Action Name", "Controller Name", new { @class = "form-signin" })
  ```

  *NOTE:* to pass class names you must use @class, because class is a reserved word


  ```
  @using (Html.BeginForm("Sign_in", "Home", FormMethod.Post, new { @class = "form-signin" }))
  {
      <h2 class="form-signin-heading">Please sign in</h2>
      <label for="inputEmail" class="sr-only">Email address</label>
      @Html.TextBox("Email address", "", new { type = "email", id = "inputEmail", @class = "form-control", placeholder = "Email address", required = true, autofocus = true })
      <label for="inputPassword" class="sr-only">Password</label>
      @Html.Password("Password", "", new { type = "password", id = "inputPassword", @class = "form-control", placeholder = "Password", required = true})
      <button class="btn btn-lg btn-primary btn-block" type="submit">Sign in</button>
  }
  ```
  ### Done!

  You now have a working sign in form

  ![picture of complete product](images/finished.png)


  # Forms and Helpers


  ## Overview

  - HTML Helpers "help" you work with HTML
  - Forms are containers that allows a user to enter information and SUBMIT it to a server
  - The action attribute tells where to send the information
  - The method attribute tells where to HTTP POST or HTTP GET when sending the information
  - The default method is HTTP GET
  - Basically, when a user sees a view it is the HTTP GET and when a user submits, it is usually an HTTP POST
  - The Get method requests a representation of a resource. IOW, they retrieve data
  - The Post method requests that the server receive something that is being sent (i.e. data)


  ## Forms

  ```html
  <form action="http://www.google.com/search" method="get">
  	<input name="first" type="text" />
  	<input type="submit" value="Search" />
  </form>
  ```

  - If you entered "greg" in the input box on the form and click on the submit button, it would result in the following url being created and sent to the browser


    `http://www.google.com/search?first=greg`
  - So the HTTP Get request takes the information (values) inside of the form and builds a query string using names and values as a pair
  - Use GET to read and POST to write

  ## Helpers

  - An HTML Helper is a method that makes views easier to create
  - The helper assists in correctly encoding attributes, building URLs, and simplifying model binding

  ```html
  @using (Html.BeginForm("Search", "Home", FormMethod.Get))
  {
  	<input type="text" name="first" />
  	<input type="submit" value="Search" />
  }
  ```

  - This BeginForm helper can be interpreted as being "How can I get to the Search Action for the Home controller?"
  - This creates the necessary routes for you thus saving you a lot of time in handling all of that code that will generate the correct URL
  - The Helper outputs HTML markup
  - Most HTML Helpers include an html attribute parameter
  - Some of the following HTML helpers that are built into the ASP.NET MVC framework are:
    - Html.BeginForm
    - Html.TextBox
    - Html.TextArea
    - Html.Password
    - Html.Hidden
    - Html.CheckBox
    - Html.RadioButton
    - Html.DropDownList and Html.ListBox (all items are displayed without drop down)
    - Html.ValidationSummary and Html.ValidationMessage


  ### Html.BeginForm

  - The BeginForm helper writes an opening form tag
  - The form uses a POST method and the request is processed by the action method for the view

  ```html
  @using (Html.BeginForm())
  {
      @Html.TextBox("Name");
      @Html.Password("Password");
      <input type="submit" value="Sign In">
  }
  ```

  - This code produces the following form element:


    ```html
    <form action="/Original Controller/Original Action" action="post">
    ```

  - There are a number of overloaded BeginForm methods which you can see here:


    https://msdn.microsoft.com/en-us/library/system.web.mvc.html.formextensions.beginform(v=vs.100).aspx


  ### Html.TextBox

  - Html.TextBox is a plain text box
  ```csharp
  //Code in the View
  @Html.TextBox("First")
  ```

  ```csharp
  //Code in the Controller
   // GET: Default
  public ActionResult Index()
  {
      ViewBag.First = "greg";
      return View();
  }
  ```

  - By giving the helper the same name as a value in the viewbag, MVC will make a connection between the two. In this case, the token "First" in the viewbag is linked to the @Html.TextBox using the same name


  ### Html.TextArea

  - Html.TextArea is a multi-line text box

  ```csharp
  //Code in the View
  @Html.TextArea("First")
  ```

  ```csharp
  //Code in the Controller
  // GET: Default
  public ActionResult Index()
  {
      ViewBag.First = "greg";
      return View();
  }
  ```

  ### Html.Password

  - Html.Password is a text box that allows for password entries and uses "*" for input

  ```csharp
  //Code in the View
  @Html.Password("First")
  ```

  ```csharp
  //Code in the Controller
  // GET: Default
  public ActionResult Index()
  {
      ViewBag.First = "greg";
      return View();
  }
  ```

  ### Html.Hidden

  - Html.Hidden is a field designed to make it easy to bind to view data or model data and also used for storing a value

  ```csharp
  //Code in the View
  @Html.Hidden("First")
  ```

  ```csharp
  //Code in the Controller
   // GET: Default
  public ActionResult Index()
  {
      ViewBag.First = "greg";
      return View();
  }
  ```

  ### Html.CheckBox

  - Html.CheckBox returns a check box that allows the user to either select it (true) or un-select it (false)
  - In this example, it grabs the value out of the viewbag for the ReceiveEmail and casts it as a boolean for the checkbox. The words "Click Me" are displayed next to the Checkbox

  ```csharp
  //Code in the View
  @Html.CheckBox("ReceiveEmail", (bool)ViewBag.ReceiveEmail) Click Me
  ```

  ```csharp
  //Code in the Controller
  // GET: Default
  public ActionResult Index()
  {
      ViewBag. ReceiveEmail = true;
      return View();
  }
  ```


  ### Html.RadioButton

  - Html.RadioButton can be used to present the user with a list of mutually exclusive options (you can only check one)
  - In this example, the radio buttons are part of a group called Gender. "Male" and "Female" are the different names of the buttons. The label "Male" is selected. Then text as used to be displayed by the buttons.

  ```csharp
  //Code in the View
  	@Html.RadioButton("Gender", "Male", true) Male<br />
  	@Html.RadioButton("Gender", "Female", false) Female<br />
  ```


  You could also do this to provide a default value dynamically:

  ```csharp
  //In the controller:
  	ViewBag.Male = false;
  	ViewBag.Female = true;

  //In the view:
   	@Html.RadioButton("Gender", "Male", (bool)ViewBag.Male) Male<br />
  	@Html.RadioButton("Gender", "Female", (bool)ViewBag.Female) Female<br />
  ```

  ### Html.DropDownList

  - Html.DropDownList can be used to present the user with a drop down list of values (accessed as strings)

  ```csharp
  //Controller
  public ActionResult Index()
  {
      List<SelectListItem> states = new List<SelectListItem>();
      states.Add(new SelectListItem { Text = "Colorado", Value = "0" });
      states.Add(new SelectListItem { Text = "Texas", Value = "1" });
      states.Add(new SelectListItem { Text = "Utah", Value = "2", Selected = true });
      ViewBag.StateNames = states;
      return View();
  }
  ```

  - This creates a variable called states of type list and adds 3 items to the list using the text and the value when selected. The state of Utah is selected initially

  ```html
  //View
  @using (Html.BeginForm("CategoryChosen", “Home", FormMethod.Get))
  {
    <fieldset>
        US States
        @Html.DropDownList("StateNames")
        <p>
            <input type="submit" value="Submit" />
        </p>
    </fieldset>
  }
  ```

  ```csharp
  // CategoryChosen Controller
   public ViewResult CategoryChosen(string StateNames)
  {
    if (StateNames.Equals("0"))
      ViewBag.messageString = "Colorado";
    else if (StateNames.Equals("1"))
      ViewBag.messageString = "Texas";
    else if (StateNames.Equals("2"))
      ViewBag.messageString = "Utah";

    return View("CategoryChosen");
  }
  ```

  ```html
  @{
      ViewBag.Title = "CategoryChosen";
   }

  <h2>CategoryChosen</h2>
  ```
  - You selected @ViewBag.messageString

  - Another Example:

  ```csharp
  // GET: Default
  public ActionResult Index()
  {
    List<string> states = new List<string>();
    states.Add("CO");
    states.Add("TX");
    states.Add("UT");
    ViewBag.StateNames = new SelectList(states);
    return View();
  }

  public ViewResult CategoryChosen(string StateNames)
  {
    ViewBag.state = StateNames;
    return View("CategoryChosen");
  }
  ```

  ```html
  //Index View
  @{
      ViewBag.Title = "Category Select";
  }

  @using (Html.BeginForm("CategoryChosen", "Default", FormMethod.Get))
  {
    <fieldset>
     US States
      @Html.DropDownList("StateNames")
      <p>
          <input type="submit" value="Submit" />
      </p>
    </fieldset>
  }

  //CategoryChosen View
  @{
      ViewBag.Title = "CategoryChosen";
  }

  <h2>CategoryChosen</h2>
  ```

  - You selected @ViewBag.state


  ### Html.ValidationSummary and Html.ValidationMessage

  - Html.ValidationSummary returns an HTML div element that contains an unordered list of validation error messages from the model-state dictionary while the Html.ValidationMessage works with a specific field in the ModelState dictionary. You can tie a model error directly to a field

  ```csharp
  //View
  @Html.ValidationMessage("FirstName")
  @Html.ValidationSummary(false)
  ```

  - In this case, the ValidationMessage works with a specific field called FirstName.  The ValidationSummary(false) invalidates the model so any errors are displayed (NOTE: This is done simply to show you how the messages work. Usually you will check the modelstate in the post)

  ```csharp
  //Controller
  [HttpGet]
  public ActionResult Index()
  {
    if (!ModelState.IsValid)
    {
      ModelState.AddModelError("FirstName", "First Name is not valid");
    }

    return View();
  }
  ```

  ## ViewBag vs. ViewData

  - The ViewBag and ViewData help you maintain data when you move from a controller to a view and can be used to actually pass the data from the controller to the view
  - ViewData is a dictionary of objects accessible via strings (which are keys)
  - ViewBag is a dynamic property and allows you to dynamically add values to the ViewBag
  - ViewData requires typecasting while ViewBag does not


    `ViewData["FirstName"]`


    is the same as


    `ViewBag.FirstName`

  ```csharp
  //Controller
  public ActionResult Index()
  {
  	ViewBag.Name = "Buck Wheat";
  	return View();
  }

  public ActionResult Index()
  {
  	ViewData["Name"] = "Buck Wheat";
  	return View();
  }

  //View
  @ViewBag.Name
  @ViewData["Name"]
  ```

  ```csharp
  //Controller
  using System;
  using System.Collections.Generic;
  using System.Linq;
  using System.Web;
  using System.Web.Mvc;
  using HelpersSampleReal.Models;

  namespace HelpersSampleReal.Controllers
  {
      public class DefaultController : Controller
      {
          public ActionResult Index()
          {
              ViewBag.Person = new Person { FirstName = "Greg" };
              return View();
          }

      }
  }


  //View
  @Html.TextBox("Person.FirstName")
  ```


  **NOTE: The Model reference was included in the controller**


  ## Strongly Typed Helpers

  - Strongly typed helpers allow you to work directly with a model to retrieve data
  - You must first include the model in the controller


    `@model HelpersSampleReal.Models.Person`

  - Once the model is in place, the VIEW can now work with the fields in the model. You can explicity type the model.field or use a lambda expression


    `@Html.TextBoxFor(m => m.FirstName)`


    OR


    `@Html.TextBox("FirstName", Model.FirstName);`

  - Strongly typed helpers can also take advantage of meta data for the model


    `@Html.Label("FirstName")`

  produces:


    `<label for="FirstName">First Name</label>`


  where the model contains:

  ```csharp
  [DisplayName("First Name")]
  public string FirstName  { get; set; }
  ```


  ### Templated Helpers

  - Using helpers such as Html.LabelFor and Html.TextBoxFor are great for displaying a value within a model. However, you don’t have much control over how the value is displayed
  - Templated helpers allows you to customize how the data is presented to the user for displaying and editing
  - The Templated helpers are:
    - Html.DisplayFor
    - Html.EditorFor
    - Html.DisplayForModel
    - Html.EditorForModel


  ### Html.DisplayFor Helper

  - Html.DisplayFor  - displays a model property using what is known as a Display

  - For example, if the model had a field called Birthdate that had a value of 08 Dec 1990 and you issued the following statement in the view:


    `@Html.DisplayFor(model => model.BirthDate)`

  - The output would be: o8 Dec 1990


  ### Html.EditorFor Helper


  - Html.EditorFor – displays a user interface for editing a model property

  - Using the same example as above with 08 Dec 1948 in the BirthDate value, if you had the following statement in the view:


    `@Html.EditorFor(model => model.BirthDate)`

  - The output would be:

  <img width="189" alt="calendar" src="https://cloud.githubusercontent.com/assets/8953261/16826851/3a56753a-493e-11e6-84bf-667fe849a211.png">


  ### Html.DisplayForModel Helper

  - HtmlDisplayForModel - displays the whole model (not just a single property) using a display template
  - This helper would display the Name of the property (or display name if set up in the model) and then the value associated with the property
  - For example:

  	`@Html.DisplayForModel()`

  	![model-complete](https://cloud.githubusercontent.com/assets/8953261/16827004/9fb0abde-493f-11e6-88dd-a38edcdb0158.png)


  - HtmlEditorForModel -displays the whole model for editing using a given editor template
  - This helper would display the Name of the property (or display name if set up in the model) and then the value associated with the property

  - For example:

  	`@Html.DisplayForModel`

  	![model](https://cloud.githubusercontent.com/assets/8953261/16826969/307556d4-493f-11e6-9eb9-3d718c9bd2ad.png)


  ### Html.ActionLink Rendering Helper

  - Html.ActionLink – renders or creates a hyperlink (anchor tag) to another controller action
  - The ActionLink methods have specific knowledge about ASP.NET MVC Controllers and actions


    `@Html.ActionLink("List Students", "Student")`

  - When this is rendered it returns:


    `<a href="/Home/Student">List Students</a>`

  - You can also tell the ActionLink to go to a specific controller and a specific action


  	`@Html.ActionLink("List Students", "ListAllStudents", "Student")`

  - When this is rendered it tells the system that if the user clicks on "List Students", that go to the ListAllStudents action in the Student controller.


  **NOTICE: You do not specify the "Controller" word on the controller input. The actual controller name is StudentController. You leave off the word "Controller"**


  ### Html.RouteLink Rendering Helper

  - Html.RouteLink – renders or creates a hyperlink (anchor tag) to another controller action
  - The Html.ActionLink renders the hyperlink tag to the specified controller action which uses the Routing API internally to generate UR while the Html.RouteLink is similar to the ActionLink but accepts a parameter for route name and does not include the parameters for Controller name and action name


    `Html.RouteLink("Click Here", new {controller= "Home", action= "Index"})`

  - It is similar to as <a> tag in Html:


    `<a href="/Home/Index">Click Here</a>`

  - The routing system looks up the route by name, and populates the URI with values from the RouteCollection you passed


  ### URL Helpers

  - URL Helpers are similar to the Rendering Helpers except that the URL helpers return the URL as a string while the Rendering Helpers actually BUILD the URL
  - There are three types of URL Helpers
    - Action
    - Content
    - RouteURL
  - The Action URL helper is exactly like the Rendering Action helper except that it is NOT returned as an anchor tag


    `@Url.Action("Browse", "Store", new { genre = "Jazz" }, null)`

  - is rendered as:


    `/Store/Browse?genre=Jazz`

  - Html.Partial
  - Html.RenderPartial
  - Html.Action
  - Html.RenderAction


  #### Action URL Helper

  - The Action URL helper is exactly like the Rendering Action helper except that it is NOT returned as an anchor tag


    `@Url.Action("Browse", "Store", new { genre = "Jazz" }, null)`

  - is rendered as:


    `/Store/Browse?genre=Jazz`

  - Html.Partial
  - Html.RenderPartial
  - Html.RenderAction


  #### Route URL Helper

  - The Route URL Helper is like the Action helper but UNLIKE the RouteLink rendering helper in that it only acceps a route name and does not allow for any arguments for a controller and action


  `@Url.RouteUrl("Default", new { Action = "About" }, Request.Url.Scheme)`


  ### Content URL Helpers

  - The Content URL Helper converts a relative application path to an absolute application path
  - For example:


    `<script src="@Url.Content("~/Scripts/jquery~1.10.2.min.js")" type = "text/javascript"></script>`

  - However, ASP.NET MVC 5 automatically resolves the ~ character when it appears the src attribute for Script, Style and Img elements


  # Annotations and Validations

  ## Contents

  - Validations
  - Annotations
  - Requited Attribute
  - DisplayName Attribute
  - StringLength Attribute
  - RegularExpression Attribute
  - How does Regex Work?
  - Regex Samples
  - Range Attribute
  - Compare Attribute
  - Error Messages
  - ModelState
  - Controllers and Validation Errors
  - Display Annotations
  - ScaffoldColumn Attribute
  - DisplayFormat Attribute
  - ReadOnly Attribute
  - DataType Attribute


  ### Validations

  - Validation logic should execute on the browser and also on the server
  - It browser validation provides the uses instant feedback
  - The server validation is because we should validate any data arriving to the server


  ### Annotations

  - Annotations are means to add the validation through the use of metadata which describes the data on a web page
  - Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users
  - Data Annotations provide server side validation
  - Attributes to use in Data Annotations:
    - Required
    - DisplayName
    - StringLength
    - RegularExpression
    - Range
    - Compare


  #### Required Attribute

  - The Required attribute validates that the user entered information within a property of the model
  - You define the validation through the use of editing the model and the [ ] (square brackets) along with the word Required

  ```csharp
  [Required]
  public string FirstName { get; set; }
  ```

  - This statement binds the required attribute to the FirstName property for the model. If the user leaves it empty or if it is NULL, a validation error is raised.
  - You would see a message like "The FirstName field is required" when the user submits the form


  #### DisplayName Attribute

  - The DisplayName is NOT a validation but it IS an attribute within a property of the model
  - This attribute defines the text we want used on form fields and validation messages  

  ```csharp
  [DisplayName("Genre")]
  public int      GenreId    { get; set; }
  ```

  - If you use in your view the following:

  ```html
  @Html.LabelFor(x => x.GenreId)

  would generate:

  <label for="GenreId">Genre</label>
  ```

  - You could also use the Display attribute and then specify the metadata you want to modify

  ```csharp
  [Display(Name = "Color Code")]
  public string Color { get; set; }
  ```

  #### StringLength Attribute

  - The StringLength attribute defines a maximum length for a string field

  ```csharp
  [Required(ErrorMessage = "An Album Title is required")]
  [StringLength(160)]
  public string Title { get; set; }
  ```

  - This statement indicates that the maximum string length for the Title property will be 160 characters
  - It also specifies that it is required and if they leave it empty or null then the error message "An Album Title is required" will be displayed
  - You can also specify the minimum number of characters that should be entered by using the MinimumLength attribute

  ```csharp
  [StringLength(255, MinimumLength = 8)]
    // OR
  [StringLength(255, MinimumLength = 8, ErrorMessage = "Invalid")]
  ```


  #### RegularExpression Attribute

  - The RegularExpression attribute invokes the regex (stands for Regular Expression) and is a special text string describing a pattern used either for searching or string matching
  - It is used to make sure only certain characters have been used for input
  - For example, if you want to try and validate a name you might assume that a name will not contain numbers of special characters and only upper and lower case alphabet values should be accepted
  - Like other attributes, the RegularExpression is expecting a parameter of type string

  	```csharp
  	[RegularExpression(@"^[0-9]{0,15}$", ErrorMessage = "PhoneNumber should contain only numbers")]

  	[RegularExpression(@"^([a-zA-Z0-9_\.\-])+\@(([a-zA-Z0-9\-])+\.)+([a-zA-Z0-9]{2,4})+$", ErrorMessage = "Please Enter Correct Email Address")]
  	```

  - Be aware that there are attributes/data types already built in that you can use in the model to ensure proper data entry. For example:

  	```csharp
  	[Email(ErrorMessage = "Bad email")]

    // OR simply specify that the property is an EmailAddress

  	[EmailAddress]
  	```

  #### How does Regex work?

  - There are some characters with special meanings:  ` \ ^ $ . | ? * + ( ) [ ] { }`
  - If you want to use any of these characters as a literal character (IOW, no special meaning) then you would need to "escape" them with a backslash: `\`.
  - NOTE: The `{ }` usually does not require the escape
  - Character set uses the `[ ]` (the hyphen within the `[ ]` acts as a range)


  	`[0-9a-fA-F]` indicates you can have a single digit, a lower case `a` through `f`, and an upper case `A` through `F`

  - A caret ^ negates the value


  	`q[^u]` indicates that you can allow the letter "q" followed by a character that is NOT a "u"

  - If the character after a hyphen is an opening bracket then it is interpreted as a subtraction operator rather than a range operator


  	`[a-z-[aeiuo]]` indicates you can have an a through z but it cannot include a vowel of a, e, i, u, or o


  	`[0-9-[0-6-[0-3]]]` indicates you can have 0 through 9 but not 0 through 6 but exclude 0 through 3 meaning you can 		have the result of 0123789

  - For more information on regex in C#, please look at


    http://www.mikesdotnetting.com/article/46/c-regular-expressions-cheat-sheet


  ##### Samples

  - Zip code (matches 12345 and 12345-5432)


  	`^\d{5}([\-]\d{4})?$`

  - US States


  	`^(?:(A[KLRZ]|C[AOT]|D[CE]|FL|GA|HI|I[ADLN]|K[SY]|LA|M[ADEINOST]|N[CDEHJMVY]|O[HKR]|P[AR]|RI|S[CD]|T[NX]|UT|V[AIT]|W[AIVY]))$`


  - Phone Number – Matches phone numbers with or with out extensions,  (555) 555-555 and (555) 555-5555 123


  	`^\+?\(?\d+\)?(\s|\-|\.)?\d{1,3}(\s|\-|\.)?\d{4}[\s]*[\d]*$`


  #### Range Attribute

  - The Range attribute specifies the minimum and maximum constraints for a numerical value

  	```csharp
    [Range(0,17)]
    public int MinorAge { get; set; }
  	```

  - The values are inclusive and can work with integers and doubles

  #### Compare Attribute

  - The Compare attribute makes sure that 2 properties on a model object have the same value
  - For example, maybe you have the user enter their email address and then enter it again as a confirmation

  ```csharp
  [EmailAddress]
  public string Email { get; set; }

  [EmailAddress]
  [Compare("Email")]
  public string ConfirmEmail { get; set; }
  ```

  - When entered, if the addresses do not match, the validator will display a message similar to " 'ConfirmEmail' and 'Email' do not match"


  ### Error Messages

  - Every validation attribute allows you to pass a parameter for ErrorMessage

  	```csharp
  	[Required(ErrorMessage="The First Name is required")]
  	public string FirstName { get; set; }
  	```

  - You can also have a single item in the string

    ```csharp
    [Required(ErrorMessage="The {0} is required")]
  	public string FirstName { get; set; }
    ```

  - The {0} item will extract the property name


  ### Model State

  - ASP.NET MVC executes validation logic during the model binding
  - The model binder automatically runs when you have a parameter associated with an action method

  ```csharp
  public ActionResult Create(Album album)

  // You can also request model binding using the UpdateModel or TryUpdateModel methods

  public ActionResult Create(Album album)
  {
  	if (TryUpdateModel(album))
  	{
  	  // …do something here
  	}
  }

  ```

  - After the model binder has updated the model properties with new values, it uses the model metadata to get all of the validators and executes the validation logic
  - Any errors are put into the Model State and the ModelState.IsValid would return a false


  ### Controllers and Validation Errors

  - When validation fails, an action generally renders the same view that posted the model values. This allows the user to see all of the validation errors and fix them

  ```csharp
  [HttpPost]
  public ActionResult UpdatePament(Order newOrder)
  {
    if (ModelState.IsValid) {
    	//…do the posting and saving
    }

    return View(newOrder);
  }
  ```


  ### Display Annotations

  - The Display attributes allows you to set the display name for the model property

    ```csharp
    [Display(Name="First Name")]
  	public string FirstName { get; set;}

  	// Through the use of this attribute you can also control the order in which the properties are displayed in the user interface. Otherwise, the properties are displayed in sequential order. This would display First Name, Last Name, and Address in that order

  	[Display(Name="First Name", Order=15001)]
  	public string FirstName { get; set;}

  	[Display(Name="Address ", Order=15003)]
  	public string Address { get; set;}

  	[Display(Name="Last Name ", Order=15002)]
  	public string LastName { get; set;}

    ```

  - The default value for order is 10,000 and the properties using the order will be displayed in ascending order


  ### ScaffoldColumn Attribute

  - The ScaffoldColumn hides a property from the HTML helpers (EditorForModel and DisplayForModel)
  - Setting the ScaffoldColumn to false would mean that the EditorForModel would no display an input or label for the field

    ```csharp
    [ScaffoldColumn(false)]
  	public string Username { get; set; }
    ```

  ### DisplayFormat Attribute

  - The DisplayFormat attribute allows you to provide various formatting options for the property and is only used with EditorFor and DisplayFor and not by the HTML helpers (i.e. TextBoxFor)

    ```csharp
    [DisplayFormat(ApplyFormatInEditMode=true, DataFormatString="{0:c}")]
  	public decimal Total { get; set; }
    ```

  - This informs the EditorFor and DisplayFor that the Total property of type decimal will use the dataformatstring of currency and that the system should apply the attribute in the EditMode of the form input


  	Output within the TextBox could be:

  	```
  	Total
  	$12.10
    ```

  ### ReadOnly Attribute

  - The ReadOnly attribute makes the property read only (imagine that!)
  - The tells the model binder to not set the property with a new value from the request

    ```csharp
    [ReadOnly(true)]
    public decimal Total { get; set; }
    ```


  ### DataType Attribute

  - The DataType attribute allows you to provide the runtime with information about the  purpose of the property

  	```csharp
  	[DataType(DataType.Password)]
  	public string UserPassword { get; set; }
  	```

  - Here are the different values you can use with the DataType attribute:

    - `CreditCard`  	Represents a credit card number.
    - `Currency`	    Represents a currency value.
    - ``Date``		      Represents a date value.
    - `DateTime`	    Represents an instant in time, expressed as a date and time of day.
    - `EmailAddress`	Represents an e-mail address.
    - `Html`		      Represents an HTML file.
    - `ImageUrl`    	Represents a URL to an image.
    - `MultilineText`	Represents multi-line text.
    - `Password`	    Represent a password value.
    - `PhoneNumber`	  Represents a phone number value.
    - `PostalCode`	  Represents a postal code.
    - `Text`		      Represents text that is displayed.
    - `Time`		      Represents a time value.
    - `Upload`	    	Represents file upload data type.
    - `Url`		        Represents a URL value.


    # How to Create a Dropdown List

    - The first thing to do is to take care of all of the database stuff (i.e. connectionString, dbContext, Models, [Table””], etc.
    - Once those are in place, creating a drop down list is easy!
    - In this sample we will just use the ViewBag. Why? Because there are not many records to worry about. The positions table only has 4 -6 records and the teams table probably only has 20-30 records.
    - If there are a lot of records then you might instead want to use something call a ViewModel which could make a strongly typed view and make your code cleaner and easier to maintain.
    - If you are working with data that doesn’t really change much (i.e. positions, teams, cities, states, titles, etc.) then a ViewBag is acceptable.

    ## 1. In your controller, add the records you want to use as the look up to the ViewBag

    ```csharp
    ViewBag.positions = db.Positions.ToList();
    ```

    ## 2. In your view, add the code necessary to display a drop down list showing the description and returning the code.

    - The code returned will be the code from the look up table that you wish to store in the main table. In our case, we will use the Positions table for the look up. We will display the Position Description but return the Position Code. The returned value (Position Code) will be the value that gets back to the main table (Players.PositionCode).

    ```html
    <div class="form-group">
        @Html.LabelFor(model => model.positionCode, htmlAttributes: new { @class = "control-label col-md-2" })
        <div class="col-md-10">
            @Html.DropDownListFor(model => model.positionCode, new SelectList(ViewBag.positions, "positionCode", "positionDesc"))
            @Html.ValidationMessageFor(model => model.positionCode, "", new { @class = "text-danger" })
        </div>
    </div>
    ```


    NOTE: Make sure you are using the DropDownListFor since it works with a model.


    # Model Within Model


    ## Getting Started

    - Create an MVC Project called CarsRUs
    - Create a database called CarOwners
    - Within that database I created 2 tables: Cars and Owners


    The Cars table looked like:

    ```sql
    CREATE TABLE [dbo].[Cars] (
        [carID]   INT          IDENTITY (1, 1) NOT NULL,
        [carName] VARCHAR (30) NOT NULL,
        [ownerID] INT          NULL,
        PRIMARY KEY CLUSTERED ([carID] ASC)
    );
    ```


    The Owners table looked like:
    ```sql
    CREATE TABLE [dbo].[Owner] (
        [ownerID]        INT          IDENTITY (1, 1) NOT NULL,
        [ownerLastName]  VARCHAR (30) NOT NULL,
        [ownerFirstName] VARCHAR (30) NOT NULL,
        PRIMARY KEY CLUSTERED ([ownerID] ASC)
    );
    ```


    Meaning that each car could only have 1 owner but 1 owner could be attached to multiple cars. Be aware that one could also create a joining table made up of the primary keys from the Cars and Owner table.


    Note that I set the carID and the ownerID as identity fields


    - Add some data to the Cars table (maybe 3 records or so for now)
    - Copy the connection string and add it to the Web.config in the connectionStrings section (If this section is not present you can just create it – see other examples posted)

    ```xml
    <add name="CarOwnersContext" connectionString="Data Source=(localDB)\v11.0;Initial Catalog=CarOwners;Integrated Security=True;Connect Timeout=15;Encrypt=False;TrustServerCertificate=False"
          providerName="System.Data.SqlClient" />
    ```


    - Create the models for the Cars

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.ComponentModel.DataAnnotations;
    using System.ComponentModel.DataAnnotations.Schema;
    using System.Linq;
    using System.Web;

    namespace CarsRUs.Models
    {
        [Table("Cars")]
        public class Cars
        {
            [Key]
            public int carID { get; set; }

            [Required(ErrorMessage = "The Car name is required")]
            [DisplayName("Car Name")]
            [StringLength(30)]
            public String carName { get; set; }

            public int? ownerID { get; set; }
        }
    }
    ```


    NOTE: I used the ? on the ownerID to allow it to be null. This is the foreign key in the Cars table that can link you to an owner if the car record has one.


    - Create the model for the Owner

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.ComponentModel.DataAnnotations;
    using System.ComponentModel.DataAnnotations.Schema;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;

    namespace CarsRUs.Models
    {
        [Table("Owner")]
        public class Owner
        {
            [Key]
            [HiddenInput(DisplayValue = false)]
            public int? ownerID { get; set; }

            [Required(ErrorMessage = "The Owner Last name is required")]
            [DisplayName("Owner Last Name")]
            [StringLength(30)]
            public String ownerLastName { get; set; }

            [Required(ErrorMessage = "The Owner First name is required")]
            [DisplayName("Owner First name")]
            [StringLength(30)]
            public String ownerFirstName { get; set; }
        }
    }
    ```


    NOTE: the HiddenInput(DisplayValue = false) attribute allows me to now worry about having a value for the primary key ownerID since it will be auto generated as an identify field. I also use the ? to allow it to be nullable. I have to do this since I am going to build a new model based upon the cars and owner models and when I want to add a new owner record and attach the ownerID to the cars ownerID field, the system will tell me that the ModelState is invalid. This allows me to get around that to add the owner record which will generate the ownerID value


    -Create a model that consists of the cars and owner models

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel.DataAnnotations;
    using System.ComponentModel.DataAnnotations.Schema;
    using System.Linq;
    using System.Web;

    namespace CarsRUs.Models
    {
        public class CarsOwner
        {
            public Cars cars { get; set; }
            public Owner owner { get; set; }
        }
    }
    ```


    NOTE: You will NOT be able to create a scaffolded controller and view based upon this model. Even if you try to create a key, the system will not be happy. We will need to create empty controllers and views and then make them strong ourselves by adding the model.


    - Modify the Global.asax file

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;
    using System.Web.Optimization;
    using System.Web.Routing;
    using CarsRUs.Models;
    using CarsRUs.DAL;
    using System.Data.Entity;

    namespace CarsRUs
    {
        public class MvcApplication : System.Web.HttpApplication
        {
            protected void Application_Start()
            {
                Database.SetInitializer<CarOwnersContext>(null);

                AreaRegistration.RegisterAllAreas();
                FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
                RouteConfig.RegisterRoutes(RouteTable.Routes);
                BundleConfig.RegisterBundles(BundleTable.Bundles);
            }
        }
    }
    ```


    - Add a new folder called DAL and create a new class for the DbContext variable

    ```csharp
    using CarsRUs.Models;
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Web;

    namespace CarsRUs.DAL
    {
        public class CarOwnersContext : DbContext
        {
            public CarOwnersContext() : base("CarOwnersContext")
            {
            }

            public DbSet<Cars> car { get; set; }
            public DbSet<Owner> owner { get; set; }
        }
    }
    ```

    - Build the project (NOTE: I would build each time you add a model – just to be safe)
    - Modify the landing page to look however you want it to look.
    - I added a link to take the user to the Index action method in the CarsController to be created

    ```html
    @{
        ViewBag.Title = "Home Page";
    }

    <div class="jumbotron">
        <h1>Car Database Tracker</h1>
    </div>

    <div class="row">
        <div class="col-md-4">
            <h2>Cars</h2>
            <p>@Html.ActionLink("List Cars", "Index", "Cars")</p>
        </div>
    </div>
    ```

    - Create a scaffolded MVC Controller – EMPTY controller called CarsController
    - Make the newly created controller Strongly typed and return the list of cars to the view
    - Also add the DbContext variable to the controller

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.Mvc;
    using CarsRUs.Models;
    using CarsRUs.DAL;
    using System.Net;
    using System.Data.Entity;

    namespace CarsRUs.Controllers
    {
        public class CarsController : Controller
        {
            CarOwnersContext db = new CarOwnersContext();

            // GET: lists
            public ActionResult Index()
            {
                return View(db.car.ToList());
            }
        }
    }
    ```


    NOTE: This method will be used to display the list of cars


    - Right mouse click within the newly created controller and choose Add View
    - Leave the name as Index and Empty (without model) and click Add
    - Modify the view to display the list of cars by making it strongly typed (@model) and adding a link that will take the user to the AddOwner action method (that we will soon create) passing the carID as a parameter

    ```html
    @model IEnumerable<CarsRUs.Models.Cars>

    @{
        ViewBag.Title = "Index";
    }

    <h2>Attach Owner to Car</h2>

    <table class="table">
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.carName)
            </th>
            <th></th>
        </tr>

        @foreach (var item in Model)
        {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.carName)
                </td>
                <td>
                    @Html.ActionLink("Add Owner", "AddOwner", new { id = item.carID})
                </td>
            </tr>
        }

    </table>
    ```

    - Go back to the CarsController and add a new action method for the AddOwner action by typing in the following code:

    ```csharp
    // GET: Owners/AddOwner/5
    public ActionResult AddOwner(int? id)
    {
        if (id == null)
        {
            return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
        }
        CarsOwner carowner = new CarsOwner();

        carowner.cars = db.car.Find(id);

        if (carowner.cars == null)
        {
            return HttpNotFound();
        }

        if (carowner.cars.ownerID != null)
        {
            return RedirectToAction("CarOwned", "Cars");
        }

        return View(carowner);
    }
    ```

    NOTE: This method will receive the car id as a parameter from the Cars Index view ActionLink and name it “id” in this controller. It will find a car record from the car table in the dbcontext and add it to the cars model within the carowner model

    ```csharp
    Carowner.cars = db.car.Find(id);
    ```


    Notice that you need to first create an object for the model carowner

    ```csharp
    CarsOwner carowner = new CarsOwner();
    ```


    If the cars ownerID field already has a value, we will redirect control to a controller we will create later called CarOwner in the CarsController (You will also create the view for CarOwner that will display a message that the car already has an owner and allow the user to go back to the list of cars in the Cars | Index)


    - Right mouse click within the AddOwner controller and choose Add | View
    - Leave the name as AddOwner with the Empty (without model) and click Add


    NOTE: This newly created view for AddOwner will display the car information and then allow the user to type in a new car owner

    - Type in the following source code for the AddOwner view:

    ```html
    @model CarsRUs.Models.CarsOwner

    @{
        ViewBag.Title = "Add Car";
    }

    <h2>Add Car</h2>

    <table class="table">
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.cars.carName)
            </th>
            <th></th>
        </tr>
        <tr>
            <td>
                @Html.DisplayFor(model => model.cars.carName)
            </td>
        </tr>
    </table>

    @using (Html.BeginForm())
    {
        @Html.AntiForgeryToken()

        <div class="form-horizontal">
            <h4>Owner for the Car</h4>
            <hr />
            @Html.ValidationSummary(true, "", new { @class = "text-danger" })
            @Html.HiddenFor(model => model.cars.carID)
            @Html.HiddenFor(model => model.cars.carName)
            @Html.HiddenFor(model => model.cars.ownerID)

            <div class="form-group">
                @Html.LabelFor(model => model.owner.ownerFirstName, htmlAttributes: new { @class = "control-label col-md-2" })
                <div class="col-md-10">
                    @Html.EditorFor(model => model.owner.ownerFirstName, new { htmlAttributes = new { @class = "form-control" } })
                    @Html.ValidationMessageFor(model => model.owner.ownerFirstName, "", new { @class = "text-danger" })
                </div>
            </div>

            <div class="form-group">
                @Html.LabelFor(model => model.owner.ownerLastName, htmlAttributes: new { @class = "control-label col-md-2" })
                <div class="col-md-10">
                    @Html.EditorFor(model => model.owner.ownerLastName, new { htmlAttributes = new { @class = "form-control" } })
                    @Html.ValidationMessageFor(model => model.owner.ownerLastName, "", new { @class = "text-danger" })
                </div>
            </div>

            <div class="form-group">
                <div class="col-md-offset-2 col-md-10">
                    <input type="submit" value="Add Owner" class="btn btn-default" />
                </div>
            </div>
        </div>
    }

    <div>
        @Html.ActionLink("Back to List", "Index")
    </div>

    @section Scripts {
        @Scripts.Render("~/bundles/jqueryval")
    }
    ```


    Notice that we made the view strongly typed to the CarsOwner model. This now allows us to access the car information and the owner information.


    We hid the following fields because we need them to validate our model:

    ```html
    @Html.HiddenFor(model => model.cars.carID)
    @Html.HiddenFor(model => model.cars.carName)
    @Html.HiddenFor(model => model.cars.ownerID)
    ```


    We did not have to include the ownerID from the owner table since in the owner model we said to make it hidden


    When the user clicks on the Post method we want to try and create a new owner and then attach the newly created ownerID field (created by the database since it was an identity field) and attach it to the car.ownerID field

    - Go back to the CarsController and add new AddOwner action method for the POST

    ```csharp
    // POST: Owners/AddOwner
            // To protect from overposting attacks, please enable the specific properties you want to bind to, for
            // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
            [HttpPost]
            [ValidateAntiForgeryToken]
            public ActionResult AddOwner( CarsOwner carsowner)
            {
                if (ModelState.IsValid)
                {
                    var entity = new Owner() { ownerFirstName = carsowner.owner.ownerFirstName, ownerLastName = carsowner.owner.ownerLastName };

                    db.owner.Add(entity);

                    db.SaveChanges();

                    int newOwner = (int)entity.ownerID;

                    using (db)
                    {
                        // make sure you have the right column/variable used here
                        var cars = db.car.FirstOrDefault(x => x.carID == carsowner.cars.carID);

                        if (cars == null) throw new Exception("Invalid id: " + carsowner.cars.carID);

                        // this variable is tracked by the db context
                        cars.ownerID = newOwner;

                        db.SaveChanges();
                    }

                    return RedirectToAction("Index");
                }

                return View(carsowner);
            }
    ```

    Notice that we said the parameter that was passed by the submit button for the post was a carsowner model


    The code checks the model to see if it passed validity checks and then creates a new object referring to the Owner class (from the model)


    The code loads up the fields in the owner model except for the ownerID since it will be auto created


    It loads the data from the carsowner model and the owner model WITHIN the carsowner model


    ```csharp
    var entity = new Owner() { ownerFirstName = carsowner.owner.ownerFirstName, ownerLastName = carsowner.owner.ownerLastName };
    ```


    Then the code adds a new record to the owner table and saves the changes

    ``` csharp
    db.owner.Add(entity);
    db.SaveChanges();
    ```


    The entity was patterned after the Owner model


    Then the code stores the newly created value for the ownerID identity field into a variable:

    ``` csharp
    int newOwner = (int)entity.ownerID;
    ```


    And using the dbcontext variable, finds the car record that has the carID we have been working with and stores a reference to it in a variable called cars

    ```csharp
    using (db)
    {
    // make sure you have the right column/variable used here
           var cars = db.car.FirstOrDefault(x => x.carID == carsowner.cars.carID);

           if (cars == null) throw new Exception("Invalid id: " + carsowner.cars.carID);

           // this variable is tracked by the db context
           cars.ownerID = newOwner;

           db.SaveChanges();
    }
    ```


    If the car record is NOT found, the system will throw an exception with an error message


    Otherwise, control will continue on and the code stores the value in newOwner (which was the newly created ownerID value) into the ownerID for the referenced car record and save the changes


    - All that is left to do is to create the CarOwned controller that will be called if the ownerID has already been assigned to a car and the CarOwned view
    - In the CarsController add the action method for CarOwned:

    ```csharp
    // GET: Cars
    public ActionResult CarOwned()
    {
        return View();
    }
    ```

    - Right mouse click in the action method and choose Add View
    - Leave the name as CarOwned and Empty (without model)
    - This view is very simple but you can make it as nice as you want:

    ```html
    @{
        ViewBag.Title = "CarOwned";
    }

    <h2>CarOwned</h2>

    The car is already owned

    @Html.ActionLink("Go back to the Car Listings", "Index");
    ```

    - The actionlink will send the user back to the Car | Index view to list the cars again
    - Save the project, Build and Run


    # Adding Authorization

    - Using the PlayBall project you created in a previous tutorial, we will now add authorization.
    - We are going to hardcode a username of an email address greg@test.com and the password of greg (case-sensitive)
    - However, as discussed in class, in a real world application you would instead want to use a database table that would hold user information. You could also use OAuth authentication and log in using credentials for another site like Google.
    - Adding authorization to a project can be done via a controller, an action method, or by specifying a role
    - This tutorial will show you the basics of attaching authorization to an action method


    ### Modify the web.config


    ```xml
    REMOVE

        <authentication mode="None" />


    AND ADD

    <system.web>
      <compilation debug="true" targetFramework="4.5.1" />
      <httpRuntime targetFramework="4.5.1" />
      <authentication mode="Forms">
        <forms loginUrl="~/Home/Login" timeout="2880" />
      </authentication>
    </system.web>
    ```

    - This states that authentication will be set up to use the authentication element in the system.web section and that the mode will be forms based authentication (works with local user credentials). An alternative to forms based is Windows authentication where the operating system’s credentials are used or Organizational authentication using cloud services like Azure.
    - The loginUrl tells ASP.net where to redirect users when they need to authenticate and in this case the system sends them to the Login action method in the Account controller
    - The 2880 timeout is specified in minutes (48 hours)
    - Now you need to set a filter for an action method or controller class which allows you to use additional logic to change the behavior of the framework.
    - There are different types of filters but we are going to focus on the Authorize filter


    #### Also modify the system.webServer section in the Web.config

    ```xml
    <system.webServer>
      <modules>

      </modules>
    </system.webServer>
    ```

    ### Comment out all of the code in the Startup.Auth.cs file after the:

    ```csharp
    public void ConfigureAuth(IAppBuilder app)
    {  /*

    And to the:
                //app.UseGoogleAuthentication(new GoogleOAuth2AuthenticationOptions()
                //{
                //    ClientId = "",
                //    ClientSecret = ""
                //});*/
            }
        }
    }
    ```

    ### Create a new view called Login in the Home Views folder for your personalized Login

    ```html
    @{
        ViewBag.Title = "Login";
    }

    <h2>Login</h2>

    @using (Html.BeginForm("Login", "Home", FormMethod.Post))
    {
        <label for="inputEmail">Email address</label>
        @Html.TextBox("Email address")
        <label for="inputPassword">Password</label>
        @Html.Password("Password")
        <button type="submit">Login</button>
    }
    ```


    ### Save and build the project


    ### Open the HomeController


    ### Create the HttpGet Login Action method

    ```csharp
    // GET: Home
    public ActionResult Login()
    {
        return View();
    }
    ```

    ### Create the HttpPost Login Action method
    ```csharp
    [HttpPost]
    public ActionResult Login(FormCollection form, bool rememberMe = false)
    {
        String email = form["Email address"].ToString();
        String password = form["Password"].ToString();

        if (string.Equals(email, "greg@test.com") && (string.Equals(password, "greg")))
        {
           FormsAuthentication.SetAuthCookie(email, rememberMe);

            return RedirectToAction("Index", "Home");

        }
        else
        {
            return View();
        }
    }
    ```

    ### Add the using clause to access the System.Web.Security module

    ```csharp
    using System;
    using System.Globalization;
    using System.Linq;
    using System.Security.Claims;
    using System.Threading.Tasks;
    using System.Web;
    using System.Web.Mvc;
    using Microsoft.AspNet.Identity;
    using Microsoft.AspNet.Identity.Owin;
    using Microsoft.Owin.Security;
    using PlayBall.Models;
    using System.Web.Security; // add this line
    ```

    ### Add the [Authorize] filter to the Index action method in the HomeController
    ```csharp
    // GET: Players
    [Authorize]
    public ActionResult Index()
    {
        IEnumerable<NewPlayer> player =
            db.Database.SqlQuery<NewPlayer>(
        "Select Team.teamID, Team.teamName, Player.playerID, " +
        "Player.playerLastName, Player.playerFirstName, " +
        "Position.positionCode,  Position.positionDesc " +
        "FROM Team, Player, Position " +
        "WHERE Team.teamID = Player.teamID AND " +
        "Player.positionCode = Position.positionCode " +
        "Order by Team.teamName, Player.playerLastName, " +
        "Player.playerFirstName");

        return View(player);
    }
    ```

    ### Save and build the project


    ### Run the project by trying to access the Index action method for the Home Controller

    # Security


    ## Contents

    - ASP.NET MVC compared to Web Forms
    - ASP.NET MVC  Security
    - Authorize Attribute
    - OpenID
    - OAuth
    - Wiki Sample
    - OpenID vs OAuth
    - OAuth 2.0 in MVC
    - Change Authentication
    - Global Authentication (1)
    - Global Authentication (2)
    - AuthorizeAttribute for Roles
    - Registering External Login Providers


    ### ASP.NET MVC compared to Web Forms

    - MVC does not have as many automatic protections as ASP.NET Web Forms to secure forms like:
    - Server Components that automatically encode displayed values to prevent XSS attacks (Cross-Site Scripting (XSS) attacks are a type of injection, in which malicious scripts are injected into otherwise benign and trusted web sites. The end user’s browser has no way to know that the script should not be trusted, and will execute the script. Because it thinks the script came from a trusted source, the malicious script can access any cookies, session tokens, or other sensitive information retained by the browser and used with that site )
    - ViewState  is encrypted and validated to prevent tampering (The ViewState indicates the status of the page when submitted to the server. The status is defined through a hidden field placed on each page with a <form runat="server"> control. The ViewState allows ASP.NET to repopulate form fields on each postback to the server, making sure that a form is not automatically cleared when the user hits the submit button.)
    - Request Validation intercepts malicious looking data (this is potentially dangerous content such as any HTML markup or JavaScript code in the body, header, query string, or cookies of the request)
    - Event Validation prevents injection attacks (Event validation checks the incoming values in a POST to ensure the values are known, good values. If the runtime sees a value it doesn’t know about, it throws an exception.)


    ### ASP.NET MVC  Security

    - MVC gives you more control over markup and how the application works
    - This means that you have more  responsibility to make sure you  are taking advantage of necessary security options like HTML encoding through HTML helpers and razor, request validation, etc.)
    - NEVER TRUST ANY DATA THE USER GIVES YOU. EVER!


    ### Authorize Attribute

    - Require a user to log in to access the application
    - Use the Authorize action filter on a controller (AuthorizeAttribute is default authorization filter included in ASP.NET MVC)
    - Authentication is verifying that users are who they say they are using a login mechanism (such as username/password, OpenID, OAuth, etc.)
    - Authorization is verifying that they user can do what they want to do with respect to your site
    - You can add the attribute to the action such as:

    ```csharp
    [Authorize]
    public ActionResult Buy(int id)
    {
       … your code here
    }
    ```

    - The authorize attribute requires that the user is logged into the site and forbids anonymous access and also allows you to specify roles and permissions
    - Put the security check as close as possible to the thing you are security


    ### OpenID

    - The ASP.NET MVC template with Individual User Accounts authentication includes the AccountController that implements authentication both for OpenID and OAuth
    - OpenID is an open standard and decentralized protocol by the non-profit OpenID Foundation that allows users to be authenticated by certain co-operating sites (known as Relying Parties or RP) using a third party service.
    - OpenID allows you to use an existing account to sign in to multiple websites, without needing to create new passwords
    - OpenID was created for federated authentication, that is, letting a third-party authenticate your users for you, by using accounts they already have. The term federated is critical here because the whole point of OpenID is that any provider can be used (with the exception of white-lists). You don't need to pre-choose or negotiate a deal with the providers to allow users to use any other account they have.OAuth could be used in external partner sites to allow access to protected data without them having to re-authenticate a user
    - OpenID is about authentication (ie. proving who you are), OAuth is about authorization (ie. to grant access to functionality/data/etc.. without having to deal with the original authentication).
    - OpenID does not require hard coding each the providers you want to use ahead of time. Using discovery, the user can choose any third-party provider they want to authenticate. This discovery feature has also caused most of OpenID's problems because the way it is implemented is by using HTTP URIs as identifiers which most web users just don't get. Other features OpenID has is its support for ad-hoc client registration using a DH exchange, immediate mode for optimized end-user experience, and a way to verify assertions without making another round-trip to the provider.


    ### OAuth

    - OAuth was created to remove the need for users to share their passwords with third-party applications. It actually started as a way to solve an OpenID problem: if you support OpenID on your site, you can't use HTTP Basic credentials (username and password) to provide an API because the users don't have a password on your site.
    - OpenID is limited to the 'this is who I am' assertion, while OAuth provides an 'access token' that can be exchanged for any supported assertion via an API
    - The most important feature of OAuth is the access token which provides a long lasting method of making additional requests. Unlike OpenID, OAuth does not end with authentication but provides an access token to gain access to additional resources provided by the same third-party service. However, since OAuth does not support discovery, it requires pre-selecting and hard-coding the providers you decide to use. A user visiting your site cannot use any identifier, only those pre-selected by you. Also, OAuth does not have a concept of identity so using it for login means either adding a custom parameter (as done by Twitter) or making another API call to get the currently "logged in" user.


    ### Wiki Sample

    ![wikisample](https://cloud.githubusercontent.com/assets/8953261/17107785/f488c904-524d-11e6-9c09-529c725e41e7.png)


    ### OpenID vs OAuth

    - Oauth  (Open Authentication) is used for delegated authorization only -- meaning you are authorizing a third-party service access to use personal data, without giving out a password. Also OAuth "sessions" generally live longer than user sessions. Meaning that OAuth is designed to allow authorization
    - i.e. Flickr uses OAuth to allow third-party services to post and edit a persons picture on their behalf, without them having to give out their flicker username and password.
    - OpenID is used to authenticate single sign-on identity. All OpenID is supposed to do is allow an OpenID provider to prove that you say you are. However many sites use identity authentication to provide authorization (however the two can be separated out)
    - i.e. One shows their passport at the airport to authenticate (or prove) the person's who's name is on the ticket they are using is them.
    - OAuth is currently better suited for authorization, because further interactions after authentication are built into the protocol, but both protocols are evolving.
    - If you would like more info please visit http://oauth.net/articles/authentication/


    ### OAuth 2.0 in MVC

    - We will focus on OAuth instead of OpenID in this course
    - Latest version is OAuth 2.0
    - OAuth allows users that have multiple accounts on the Internet (Google, FaceBook, etc.) to share those resources between websites (i.e. logging into one and sharing credentials with another)
    - By using OAuth a user can log into Facebook using the userid and password and then have access to resources in Facebook to use in their ASP.NET MVC site through registered login providers
    - Support for OAuth is provided in MVC through the use of creating a site using the Internet Application template


    ### Change Authentication

    - In an MVC application you have 4 levels of authentication
      - No Authentication – No log on pages will be created
      - Individual User Accounts - Enables a user to register an account, by creating a username and password on the site or by signing in with social providers such as Facebook, Google, Microsoft Account, or Twitter
      - NOTE: The default data store for user profiles in ASP.NET Identity is a SQL Server LocalDB database, which you can deploy to SQL Server or Azure SQL Database for the production site.
      - Organizational Accounts - Configured to use Windows Identity Foundation (WIF) for authentication based on user accounts in Azure Active Directory or Windows Server Active Directory
      - Windows Authentication -  Uses the Windows Authentication IIS module for authentication. The application will display the domain and user ID of the Active directory or local machine account that is logged into Windows but won't include user registration or log-in UI. This option is intended for Intranet web sites
      - NOTE: If you choose this option then the Startup.Auth.cs will NOT be included in the project and no authentication middleware will be configured
    - The AccountController handles login and registration but does not restrict access


    ### Global Authentication (1)

    - You can place the AuthorizeAttribute attribute on actions or entire controllers to prevent unauthorized access so that attempting to access a restricted controller action when you're not authorized redirects you to login
    - If you don’t want to put the AuthorizeAttribute on every controller you can create a base class

    ```csharp
    namespace InstantMonkeysOnline.Controllers
    {
        [Authorize]
        public class AuthorizedController : Controller
        {
        }
    }
    ```

    - AND then have other controllers inherit from this class:
    ```csharp
    public class HomeController : AuthorizedController
    {
        public ActionResult Index()
        {
            ViewBag.Message = "Modify this template to jump-start your ASP.NET MVC application.";

            return View();
        }        
    }
    ```

    ### Global Authentication (2)


    - Another option is to apply an action filter to all actions in your application
    - Modify the FilterConfig.cs file in the App_Start to include  

    ```csharp
    public static void RegisterGlobalFilters(GlobalFilterCollection filters)
    {
    	filters.Add(new System.Web.Mvc.AuthorizeAttribute());
    	filters.Add(new HandleErrorAttribute());
    }
    ```

    - The problem with this is that whenever you create a controller, it will restrict access to the entire site including the AccountController (IOW, a user would have to log in before being able to access the login site – not going to work)
    - You can use the `[AllowAnonymous]` attribute on any method to opt out of authorization
    ```csharp
    [AllowAnonymous]
    Public ActionResult Login(string returnUrl)
    {
    	ViewBag.ReturnUrl = returnUrl;
    	return View();
    }
    ```

    ### AuthorizeAttribute for Roles

    - This attribute allows you to prevent anonymous access to a controller or controller action

    ```csharp
    [Authorize(Roles=“Administrator”)]
    public class StoreManagerController : Controller
    ```

    - You can also provide multiple roles

    ```csharp
    [Authorize(Roles=“Administrator, SuperAdmin”)]
    public class StoreManagerController : Controller
    ```

    - You can also specify users and roles

    ```csharp
    [Authorize(Roles=“Administrator, SuperAdmin”, Users=“Greg,Rodney,Scott”)]
    public class StoreManagerController : Controller
    ```


    ### Registering External Login Providers

    - In the App_Start\Startup.Auth.cs you need to explicitly enable external sites for login
    - This is where you place all authentication provider information in the ConfigureAuth method:

    ```csharp
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseExternalSignInCookie(DefaultAuthenticationTypes.ExternalCookie);
        app.UseFacebookAuthentication(appId: “Your App ID", appSecret: “Your App Secret");
    }
    ```

    - Here is a link of Facebook permissions https://developers.facebook.com/docs/facebook-login/permissions/v2.2
    - Here is a link of Google permissions http://www.oauthforaspnet.com/providers/google/guides/aspnet-mvc5/ (this one would help you with your homework)
    - However, the danger in this is that if your site is ever compromised, an attacker could subvert the login to your site or capture the user’s login information
    - Stick with well known trusted login providers


    # User Identity

    How to extract email and password in the Login Controller


    ```csharp
    [HttpPost]
    public ActionResult Login(FormCollection form, bool rememberMe = false)
    {
        String email = form["Email address"].ToString();
        String password = form["Password"].ToString();

        var currentUser =
            db.Database.SqlQuery<Users>(
        "Select * " +
        "FROM Users " +
        "WHERE UserID = '" + email + "' AND " +
        "UserPassword = '" + password + "'");

        if (currentUser.Count() > 0)
        {
            FormsAuthentication.SetAuthCookie(email, rememberMe);

            return RedirectToAction("Index", "Home");

        }
        else
        {
            return View();
        }
    }
    ```


    Then in the Get for an Index method to display all of the records from a model/table I access the User.Identity.UserName field which is handled by Microsoft due to our authorization that we set up on that action:

    ```csharp
    // GET: Players
    [Authorize]
    public ActionResult Index()
    {
        IEnumerable<NewPlayer> player =
            db.Database.SqlQuery<NewPlayer>(
        "Select Team.teamID, Team.teamName, Player.playerID, " +
        "Player.playerLastName, Player.playerFirstName, " +
        "Position.positionCode,  Position.positionDesc " +
        "FROM Team, Player, Position " +
        "WHERE Team.teamID = Player.teamID AND " +
        "Player.positionCode = Position.positionCode " +
        "Order by Team.teamName, Player.playerLastName, " +
        "Player.playerFirstName");

        ViewBag.UserName = User.Identity.Name;

        return View(player);
    }
    ```

    - When the Index action method is executed, the first thing that happens is that Microsoft sends a request to see if the user has already been authenticated. If not, it will go to the Login action method (as done in CheckPoint #5).
    - The login view is displayed and the user types in the email address and the password and clicks on the Login button. Control is sent to the Post method for the view and SQL query is used to find the record. Without getting into a ton of details you will not need to finish Project 2 or the Final exam, Microsoft has set up an identity model in your program when you create an MVC project. If you look under the Models you will actually see an Identity model. The system uses this model to store information from the login view through the account controller that was also created for you.
    - In the action method above, I access the Microsoft set up User field and grab the Identity.Name and store it to the Viewbag. This way I can have access to the user name on every view through the use of the User.Identify.Name
    - In the view you could do the following:

    ```html
    @model IEnumerable<PlayBall.Models.NewPlayer>
    @{
        ViewBag.Title = "Home Page";
    }

    <div class="jumbotron">
        <h1>NBA Players</h1>
    </div>
    <p>
        Hello @ViewBag.UserName
    </p>
    <table class="table">
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.teamName)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.playerLastName)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.playerFirstName)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.positionDesc)
            </th>
            <th></th>
        </tr>

        @foreach (var item in Model)
        {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.teamName)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.playerLastName)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.playerFirstName)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.positionDesc)
                </td>
                <td>
                    @Html.ActionLink("Edit", "Edit", new { id = item.playerID })
                </td>
            </tr>
        }

    </table>
    ```

    # Return to Previous URL

    ## Write the Get (route map handles the parameter)

    ```csharp
    // GET: Home
    public ActionResult Login(string ReturnUrl)
    {
        ViewBag.ReturnUrl = ReturnUrl;
        return View();
    }
    ```

    ## Write the Post
    ```csharp
    [HttpPost]
    public ActionResult Login(FormCollection form, bool rememberMe = false)
    {
        String email = form["Email address"].ToString();
        String password = form["Password"].ToString();

        var currentUser =
            db.Database.SqlQuery<Users>(
        "Select * " +
        "FROM Users " +
        "WHERE UserID = '" + email + "' AND " +
        "UserPassword = '" + password + "'");

        if (currentUser.Count() > 0)
        {
            FormsAuthentication.SetAuthCookie(email, rememberMe);
            return Redirect(form["ReturnURL"].ToString());

        }
        else
        {
            return View();
        }
    }
    ```

    ##  Modify the View hiding the URL from the ViewBag

    ```html
    @{
        ViewBag.Title = "Login";
    }

    <h2>Login</h2>

    @using (Html.BeginForm("Login", "Home", FormMethod.Post))
    {
        @Html.Hidden("ReturnURL", new { String = ViewBag.ReturnURL })

        <label for="inputEmail">Email address</label>
        @Html.TextBox("Email address")
        <label for="inputPassword">Password</label>
        @Html.Password("Password")
        <button type="submit">Login</button>
    }
    ```


    # Tutorial Explanation

This tutorial explains the syntax used in the generated scaffolding code resulting from the EF Tutorial.


## Teams Index View

- `@Html.DisplayNameFor` (extracts the column name out of the table i.e. teamName)
- `@Html.DisplayFor` (extracts the column text out of the table i.e. Utah Jazz)
- `@Html.ActionLink` (text to display as a link, controller to call (Edit in the Teams controller), what data to pass the controller (teamID)

![indexviewmodels](https://cloud.githubusercontent.com/assets/8953261/17080633/858d5624-50f3-11e6-9d6a-0d47e58a1930.png)

![displaynamefortablenameview](https://cloud.githubusercontent.com/assets/8953261/17080637/a0ca2bd8-50f3-11e6-96e8-f4b508face6c.png)

![foreachindexview](https://cloud.githubusercontent.com/assets/8953261/17080638/ad1561fa-50f3-11e6-8d1f-2323ead8881c.png)


## Teams Controller

![teamscontroller](https://cloud.githubusercontent.com/assets/8953261/17080639/c17c6c60-50f3-11e6-891b-7e83be067cd8.png)

![indexgetcontroller](https://cloud.githubusercontent.com/assets/8953261/17080643/da99ab04-50f3-11e6-98ff-c447dc2079cc.png)


In the Index View, if you clicked on the Edit action in the View it would take you to the Edit ActionResult in the Team Controller

**NOTE: This is in the Index View**


![editlinkview](https://cloud.githubusercontent.com/assets/8953261/17080644/f537a0ec-50f3-11e6-8cd7-7f5555deae53.png)


### Edit GET Action Method

![editgetcontroller](https://cloud.githubusercontent.com/assets/8953261/17080648/0999dc4e-50f4-11e6-9f7a-768e42a2bf2c.png)


### Edit POST Action Method


![editpostcontroller](https://cloud.githubusercontent.com/assets/8953261/17080654/21a371a6-50f4-11e6-90b6-5528493a0591.png)


### Create POST Action Method

![createpostcontroller](https://cloud.githubusercontent.com/assets/8953261/17080656/3885ca2c-50f4-11e6-9306-8f1c6854eea2.png)


### Delete View


![htmlbeginformview](https://cloud.githubusercontent.com/assets/8953261/17080659/505ab5e0-50f4-11e6-88be-9b8ebe12c401.png)


### Delete GET Action Method

![deletegetcontroller](https://cloud.githubusercontent.com/assets/8953261/17080661/69086f9c-50f4-11e6-85fd-af56e12e01aa.png)


### Delete POST Action Method

![deletepostcontroller](https://cloud.githubusercontent.com/assets/8953261/17080662/81bdd7f2-50f4-11e6-8100-5254bc165f37.png)


# Instructions To Add Database

1. Add Connection (Server Explorer)
2. Add Connection String to web.config
3. Rename Connection String to DatabaseNameContext
4. Make Models
  - (don't forget ? on ints that can be null)
	- [Table("TableName")]
	- [Key]
5. Update Global.aspx
  - Database.SetInitializer<DatabaseNameContext>(null);
  - using Models
  - using DAL
  - using System.Data.Entity
6. Add Folder called 'DAL'
7. Add Class DatabaseNameContext : DbContext to DAL folder
	 - using Models
	 - using System.Data.Entity
   -	Add method public DatabaseNameContext : base	("DatabaseNameContext")
