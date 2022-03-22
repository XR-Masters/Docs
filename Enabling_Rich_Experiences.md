# Enabling Rich Experiences with Java Scripting on XRM Platform

XRM Platform uses **Unity** to allow creators to create and publish content on the platform. We are utilizing Unity's **Asset Bundle** technology to do that. But this technology doesn't allow dynamically linking **C# scripts** to application and that is why we integrated **JavaScript** support for adding custom behaviours to your assets.

# Sections
- [Create index file](#creating-index-file)
- [Start and Update functions](#start-and-update-functions-in-js)
- [Accessing root content gameObject](#accessing-root-content-gameobject)
- [Accessing root content transform](#accessing-root-content-transform)
- [Accessing any other gameObject](#accessing-any-other-gameobject)
- [Getting components](#getting-components)
- [Detect user taps](#detect-user-taps)
- [Network calls](#network-calls)

## Creating index File
First step for enabling rich experiences is that you need to include **index.js** file to your asset when exporting your **unity package** from unity.

> You can refer [here](https://github.com/XR-Masters/Docs/blob/main/Create%20Content.md) if you don't know how to create and export your content to XRM Platform

## Start and Update functions in JS
Like Unity's C# there are two important functions predefined for you in **JavaScript** and these are **start** and **update** functions. **start** is good for initializing your data like finding reference to some other child gameObjects within your asset, or getting reference to some unity defined components.

> Unlike Unity start and update functions should all be lowercase otherwise your functions won't be called!
```js
function start(){
    //here you can initialize your required parameters like getting reference to other gameObjects
    //find('my-child-object-name');
}
function update(){
    //this will be called every unity frame use it wisely
}
```

### Accessing root content gameObject

In **JavaScript** there is predefined variable which called **gameObject**. If you access this, JS will return reference to your content root gameObject.

```js
function start(){
    //rootName will be resolved as obj at runtime
    let rootName = gameObject.name;
}
```

### Accessing root content transform

In **JavaScript** there is predefined variable which called **transform**. If you access this, JS will return reference to your content root transform component.

```js
function start(){
    //same api as C#, you just need to be a bit more specific when accessing UnityEngine related code
    transform.localScale = new UnityEngine.Vector3(0.6, 0.6, 0.6);
}
```
### Accessing any other gameObject

If you want to access any other gameObject which is child of your root object, you can call 
```js 
find('gameObject-name');
``` 
where 'gameObject-name' should be the name of the gameObject which you are searching for.
This will return to you the gameObject if the given name exist within your asset hierarchy.

```js 
var backLeftFootGO = find('back-left-foot');
if(backLeftFootGO != null){
    backLeftFootGO.SetActive(false);
}
``` 
> **find** function can be used to find any gameObject within your asset hieararchy no matter nesting level, it will always return first gameObject found with the given name

![](https://public.3.basecamp.com/p/1ZnDB2s2ux9nVayAw5G1derx/upload/download/find-function.JPG)

### Getting Components

For example if you look at the image below, we want to get **Animator** component from gameObject **Sphere**

![](https://public.3.basecamp.com/p/bsrQVwaaRCUaKPN7MAM82fkw/upload/download/get-component.JPG)

> Please refer to [Unity Docs for UnityEngine.Animator](https://docs.unity3d.com/ScriptReference/Animator.html) to see available methods
```js 
function start(){
    //Get a reference to gameObject Sphere and call GetComponent on it to access animator component
    var sphere = find('Sphere');
    var animator = sphere.GetComponent(UnityEngine.Animator);
    //now you can call any existing method on animator like C#
    
    //animator.Play('myClipName');
    //animator.SetBool(false);
    //animator.SetTrigger('myTriggerName');
    //Like in C# you can access animator's gameObject and transform also
    //var animatorGO = animator.gameObject;
    //var animatorTransform = animator.transform;
}
``` 

### Detect user taps

If your content has a any kind of collider in it's hierarchy, this collider can catch user input like clicking or tapping to your asset. When user taps on to your asset a special function named **onPointerClick** within your **JavaScript** file will be trigged if it is declared by you.

You can declare this function like below
```js 
function onPointerClick(){
    //do something here
}
``` 

You can use this function add interactability to your asset like changing asset's color or changing it's size.

For example we can change asset color by using this function like this
```js 
//this will be populated on start function
var material;

function start(){
    //First I will get reference to material object which color change happen
    var meshRenderer = gameObject.GetComponent(UnityEngine.MeshRenderer);
    material = meshRenderer.sharedMaterial;
    //Now we have found reference to material object which we want to change it's color
}

function onPointerClick(){
    //User tapped on to asset
    //Calculate random color
    var r = UnityEngine.Random.Range(0, 1);
    var g = UnityEngine.Random.Range(0, 1);
    var b = UnityEngine.Random.Range(0, 1);
    //create color
    var color = new UnityEngine.Color(r,g,b);
    //assign color to material
    material.color = color;
    
    //notive every call is same with C# API just we are a bit more specific when we are using UnityEngine's code
}
``` 
### Network Calls

Being able to code your own behaviour is good, but making it synchronized between all connected users even better.

When a user taps on to your asset this just happens on their device. Luckly we are supporting **networkCall()** api so that you can develop an experience shared between all connected users.

**networkCall()** is a predefined function which accepts a string and a javaScript object.
string should be the name of the function within your index.js file
javaScript object can be any javaScript object and can contain any value or values. This value can be null

```js 
//Here is an example of using networkCall API without parameters
function onPointerClick(){
    //myNetworkFunction will be triggered on all connected users as well as user tapped on the content
    networkCall('myNetworkFunction');
}

function myNetworkFunction(){
    transform.Rotate(new UnityEngine.Vector3(0, 45, 0), UnityEngine.Space.Self);
}
```

```js 
//Here is an example of using networkCall API with parameters
function onPointerClick(){
    //myNetworkFunction will be triggered on all connected users as well as user tapped on the content
    networkCall('myNetworkFunction', {
        num: 5,
        str: 'string'
    });
}

function myNetworkFunction(params){
    transform.localScale = new UnityEngine.Vector3(params.num, 1, params.num);
    gameObject.name = params.str;
}
```

Let's make previous example synchronized between all connected users. We can use **networkCall()** function to trigger any function on other users phones. 

```js 
//this will be populated on start function
var material;

function start(){
    //First I will get reference to material object which color change happen
    var meshRenderer = gameObject.GetComponent(UnityEngine.MeshRenderer);
    material = meshRenderer.sharedMaterial;
    //Now we have found reference to material object which we want to change it's color
}

function onPointerClick(){
    //User tapped on to asset
    //we are calculating random r,g,b values here and passing those values to setColor function via networkCall api
    //so all other users will have same values for r,g,b
    
    var red = UnityEngine.Random.Range(0, 1);
    var green = UnityEngine.Random.Range(0, 1);
    var blue = UnityEngine.Random.Range(0, 1);
    networkCall('setColor', {
        r: red,
        g: green,
        b: blue
    });
    
    /*
    notice we are not triggering onPointerClick function on the network because this function 
    generates random color and we want to set same color for all users.
    */
}

function setColor(params){
    material.color = new UnityEngine.Color(params.r, params.g, params.b);
}
``` 
