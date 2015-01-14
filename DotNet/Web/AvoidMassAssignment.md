Copied from http://odetocode.com/blogs/scott/archive/2012/03/11/complete-guide-to-mass-assignment-in-asp-net-mvc.aspx
One of the scenarios that I always demonstrate during an ASP.NET MVC class is how to create a mass assignment vulnerability and then execute an over-posting attack. It is a mass assignment vulnerability that led to a severe problem on [github](https://github.com/blog/1068-public-key-security-vulnerability-and-mitigation) last week.

Let's say you have the following model.

When you want to let a regular user change their first name, you give them the following form.

There is no input in the form to let a user set the IsAdmin flag, but this won't stop someone from crafting an HTTP request with IsAdmin in the query string or request body. Maybe they saw the "IsAdmin" name somewhere in a request displaying account details, or maybe they just got lucky and guessed the name.

![composing the attack](http://odetocode.com/Blogs/images/odetocode_com/Blogs/scott/Windows-Live-Writer/Avoiding-Mass-Assignments-in-ASP.NET-MVC_1416F/image_3.png)

If you use the MVC model binder with the above request and the previous model, then the model binder will happily move the IsAdmin value into the IsAdmin property of the model. Assuming you save the model values into a database, then any user can become an administrator by sending the right request. It's not enough to leave an IsAdmin input out of the edit form.

Fortunately, there are at least 6 different approaches you can use to remove the vulnerability. Some approaches are architectural, others just involve adding some metadata or using the right API.

### Weakly Typed Approaches

The [Bind] attribute will let you specify the exact properties a model binder should include in binding (a whitelist).

Alternatively, you could use a blacklist approach by setting the Exclude parameter on the attribute.

If you prefer explicit binding with the UpdateModel and TryUpdateModel API, then these methods also support whitelist and blacklist parameters.

### Strongly Typed Approaches

TryUpdateModel will take a generic type parameter.&nbsp; You can use the generic type parameter and an interface definition to restrict the model binder to a subset of properties.

This assumes your interface definition looks like the following.

Of course, the model will also have to implement the interface.

There is also a [ReadOnly] attribute the model binder will respect. ReadOnly metadata might be want you want to use if you never want to bind the IsAdmin property. (Note: I remember ReadOnly not working in MVC 2 or MVC 1, but it is working in 3 & 4 (beta)).

### An Architectural Approach

One of many architectural approaches to solve the problem is to always put user input into a model designed for user input only.

In this approach you'll never bind against business objects or entities, and you'll only have properties available for the input you expect. Once the model is validated you can move values from the input model to the object you use in the next layer of software.

Whatever approach you use, remember to treat any data in an HTTP request as malicious until proven otherwise.