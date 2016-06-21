# Skinny Controllers

## TL;DR;

Fat controllers aren't the problem. The problem is that controllers handle too many kinds of requests.


## What's wrong with MVC

There's this notion in MVC frameworks that controllers should be skinny.

```
import ListUsers from 'somewhere/list-users'
import UpdateUser from 'somewhere/update-user'

class UserController {
  
  index (tenant) {
    var result = ListUsers(tenant)
    return UserListResponse(result)
  }

  update (user) {
    var result = UpdateUser(user)
    return UserUpdateResponse(result)
  }

  // etc...
}


```

What a useless chunk of code. It's boilerplate for the sake of boilerplate. If your controller does nothing more than take some parameters, pipe them through to another function/object, and render the response, it shouldn't exist. By the way, this is *exactly* what many people think a controller should be.

In most frameworks, controllers expose many methods/functions: Create, Show, Update, List, Delete, etc. Often, you'll find controllers that have some private helper methods (e.g. `send_email_about_user_updates`) that are called by only one or two of the CRUD action methods. Or, you'll have something like this: `before_filter :only_show_viewable_step, only: [:show]` which is validation code that is restricted only to the show method.

Does anyone else think this smells funny?

It does. When you have a list of methods/properties that are related, and used together, those things should be their own entity.

## Controllers should do one thing (create *or* update *or* delete...)

The problem is that all controllers do too many things.

A better approach would be to have controllers that look something like this:

```
// PUT /{tenant}/users/{user_id} gets mapped to this controller
module UpdateUserController {
  
  run (user) {
    validate(user)
    |> persist(user)
    |> send_notifications
  }

  validate (user) {
    // Stuff and whatnot...
  }

  persist (user) {
    // Stuff and whatnot
  }

  send_notifications (result, error) {
    // Emails, SMS, caching, response rendering, etc
  }
}

```

Yeah. It's not a skinny controller. But everything that has to be done in order to perform "UpdateUser" is found here. That's one concern, in my opinion, and things related to it should be logically grouped. No need to jump around through a whole bunch of files. No methods that exist only for a small percentage of use cases. This object doesn't handle get, delete, post, put, etc. Just put, and everything required for "PUT USER" is here.


## Controllers should be highly testable. 

Someone may comment that putting logic in controllers makes it hard to test. If controllers are hard to test, they're implemented incorrectly. See Phoenix for a great example of testabilitiy done right. (Though they still mix too many concerns into their controllers.)


## Controllers should do most of the validation.

If you're sticking validation in your model, it's in the wrong place. 

Validation is something that belongs at the external edge of your system. When data first enters your system, it should be validated *there*. Once inside, it should be safe to assume that authorization and validation have been done. When you're looking at an internal method/function, you don't want to wonder, "Has the data passed to me been validated yet? Is it safe to proceed? Who knows?" You should be able to assume that all data has been validated as early in the process as is possible. That means at the controller boundary.

Another reason not to put validation on the model is that validation rules can vary based on context. Rules around create are often slightly different from those around update. The rules may vary based on *who* is taking the action, etc. Models should really just represent the shape of data. That's what a model is for. As for whether data is valid, can be, should be updated in a certain way? That's a separate concern.

Now, if several system entry points (e.g. create and update) share a large number of validation rules, validation should obviously be moved into a shared validation module that can be invoked from all relevant entry points.


## Controllers should be responsible for sending the response

Once a request has been processed, how should we respond? The answer to that question belongs in the controller.

Maybe the answer is: send HTML back to the browser. Maybe it's: Push JSON out over some websockets. Maybe its: Send a few emails and an SMS or two. Maybe it's a combination of these things. Either way, it's all response logic, and the controller should kick it off.

Again, if you find the same response code happening in various controllers, abstract it out into a response pipeline or factory or whatever, but invoke said factory from the controller.


## Controllers are *the* coordinators for your system

If you want to know, "What happens when I take action X?" The answer should be found in a controller. The controller may invoke other modules in order to do a lot of its work. Preferably, such invocations should be explicit so that you can look at the controller code and reason about everything that needs to happen (at a macro level and sometimes, for simple actions, at a micro level, too).

Controllers represent the boundary-- the external interface-- of your system. As such, the entire chain of events that pertain to a request should be coordinated by and visible from a controller (at least in macro form):

```
Request ->
  Authenticate ->
  Authorize ->
  Validate ->
  Persist (to one or more stores) ->
  Build Response ->
  Notify of Result ->
    (Render HTML, JSON, whateer, send emails, SMS, websockets, etc)
```

## Conclusion

Maintainability is inversely proportional to the number of jumps one needs to take in order to understand the flow of data through a system. Typical MVC systems produce a *lot* of jumps-- often implicit (magic)-- per request. Some of this is due to the skinny controller mantra, which is itself due to the fact that controllers handle too many kinds of requests (two is too many). If we restructure MVC a bit to have a single fat controller per action, we can produce actions which are easier to reason about. Each controller is concerned with a single action and all that needs to be coordinated for the success of that action.

Amirite? Dunno. This is just a thought experiment. Hopefully soon, I'm going to take a leisurely workday or two to refactor some real world code and test this theory.
