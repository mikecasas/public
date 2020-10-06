TOC

Example of using the ```ServiceBase``` to Post an object and return a differnt object.

* Assuming there are two models a ```Cat``` and a ```Dog```

1) In IService, add a new method

```
 Task<Dog> MyMethodNameAsync(Models.Cat Cat);
```

2) In the class that implements IService, implement the method

```
    public async Task<Dog> MyMethodNameAsync(Models.Cat Cat)
    {
        string uri = CreateAuthorizationPolicyUrl($"{Apiurl}/postthedog", Cat.ModuleId);
        return await PostJsonAsync<Models.Cat, Dog>(uri, Cat);            
    }

```

The first parameter of the PostJsonAsync method is the input type, and the second parameter is the type wanting to be returned.

The ```postthedog``` is the route name on the method on the controller.

3) In the controller, add the method to handle the route. Note the addition of the route name so as not to conflict with other Post methods in the controller. Also the parameters as being picked up from the ```body``` and not the Url. See https://docs.microsoft.com/en-us/aspnet/web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api#using-frombody


```
// POST api/<controller>
[HttpPost("postthedog")]
[Authorize(Policy = "EditModule")]
public Models.Dog PostTheDog([FromBody] Models.Catty Catty)
{
    var Dog = new Dog();

    Dog.BreedName = "Doberman";

    return Dog;
}
```