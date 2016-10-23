# Request Response Layer - .NET standard

With this layer you'll be able to create a web API. This package can be used as middleware as a replacement for MVC.

## Installation
Add the nuget package `rrl.core` to your project. In your registration on the place where you'd normally register MVC you call `app.UseRrl();`.  

## Request
Every call you'll want to make avaible for the API must inherit from our base request class `Request`. On the request you'll define the Url and method on which you'd want the request to listen.
```CSharp
namespace MyApp
{
    [Url("api/home/{Id}", Method.GET)]
    public class UserRequest: Request
    {
        public int Id { get; set; }
    } 
}
```

## Response
Every call you make must have a response. This response is defined in a base response class `Response`.
```CSharp
namespace MyApp
{
    public class UserResponse: Response
    {
        public string Name { get; set; }
    }
}
```

## Handle
With a request and response in place we can create a handler which has to implement a handle function. We do this by inheriting the abstract base class `Handler`.
```CSharp
namespace MyApp
{
    public class UserHandler: Handler<UserRequest, UserResponse>
    {
        public override UserResponse Handle(UserRequest request)
        {
            // fake method
            var user = DoDbCallToGetUser(request.Id);
            return new UserResponse
            {
                Name = user.Name
            };
        }
    }
}
```

## Dependency Injection friendly
Register services, ... And inject them into your handler.

```CSharp
namespace MyApp
{
    public class UserHandler: Handler<UserRequest, UserResponse>
    {
        private readonly IUserRepository _userRepository;
        // inject stuff
        public UserHandler(IUserRepository userRepository)
        {
            _userRepository = userRepository;
        }
        public override UserResponse Handle(UserRequest request)
        {
            var user = _userRepository.GetById(request.Id);
            return new UserResponse
            {
                Name = user.Name
            };
        }
    }
}
```
