# CSharp-.NET-Promises
If you have used promised in JavaScript, then you probably know how awesomely efficient they are making code beautiful, while still gracefully executing operations that usually take a long time.

This code library is my attempt at recreating JavaScript promises, which i have come to love and enjoy while using javascript, in Microsoft .NET

Here's what I've come up with:

####What are Promises in Programming?
Promises are based on deference (is that a word?) which is the ability to defer or delay execution of some code, until some other code has been completed, while still leaving the processor free to do other stuff. In programming, a promise is usually a function, code block, method or code statement that you wish to execute asynchonously. You could then specify what the processor should do if the code executes successfully, or how to manipulate the result of the code, or even what should happen if an error occurs while resolving the promise.

####Updates (Generics)
C# Promises is now makes use of Generics! Hooray! Now, you can predict/specify the returned promise value type, unlike previously when dynamic typing was our only hope.

####The Promise Class
This is perhaps the most basic Promise class you'll find. It consists of attributes such as:

**state** - which could be one of Pending, Fulfilled or Rejected

**current** - the Thread on which the Promise will be resolved

**success** - is an Action (method object) that if set, executes when the promise resolves successfully. It takes a single argument which is the result of the Promised method

**then** - is a List of Action objects, which will all execute in order after the success function has been executed. It takes a single argument which is the result of the Promised method

**done** - is an Action

**error** - this is an Action object that if set, will execute when an error occurs while trying to resolve the promise

**work** - a Func object which contains the method that the Promise resolves

**timeout** - an integer ... if specified, the Promise will be rejected after the time out

####Using CSharp Promises

Perhaps the easiest way to create a Promise is:
```
  Promise<bool>.Create(() => {
    //Enter Code Here
    return true;
  });
```

####Promisify your Methods

Another way to do this is to Promisify your Methods 
```
  int Add (int x, int y) {
    return x + y;
  }
  
  //the above method becomes
  
  int AddAsync (int x, int y) {
    return new Promise<int>(() => add(x, y));
  }
  
  //this could now be used as
  
  AddAsync(12345, 54321).Success((int result) => {
    Console.WriteLine(result);
  });
```

####Chaining Promise Functions

The Public Methods in the Promise Class all return the Promise Object, so you can chain Methods, such as 

```
Promise<string>.Create(() => {
  return Api.Get("https://www.google.com.ng");
}).Success((string htmlresult) => {
  Console.WriteLine(htmlresult);
}).Then((string result) => {
  result += "<p>Hello World</p>";
}).Then((string result) => {
  result += "<p>My name is Michael</p>";
}).Error((Exception ex) => {
  Debug.WriteLine(ex.Message);
  throw ex;
}).Done(() => {
  Console.WriteLine("GET from Google.com is completed");
});
```

####C# Promise Methods

Its methods include:
####

**Promise(Func<T>)** A constructor that takes a function/delegate as an argument. The function argument needs to return the type passed into the Promise.
```
  Promise<int[]> promise = new Promise<int[]>(() => getPrimeNumbers(1500));
```

**Promise<T>.Create(Func<T>)** A static method that returns a new Promise (wanna call it a factory method? feel free!)

**Wait()** Do you want to wait for the Promise to execute synchronously? We got you covered.

**Success(Action<T>)** A method that takes in the function to be executed if the Promise resolved successfully. It stores this in the success Action variable

**Then(Action<T>)** A method that takes in a function to be executed aftet the Success function is executed. You can chain multiple Then Functions and have then execute in order. It adds the function argument to the action variable

**Done(Action<T>)** A method that takes in a function to be executed last regardless of the outcome of the Promise. Whether a Promise is successful or fails, the function argument will execute if specified

**Error(Action<Exception>>)** A method that takes in a function argument to be executed in the event of an error while resolving the promise

**SetTimeOut(int)** Lets you specify a timeout in milliseconds to delay the Promise before execution

####C# Promise State
The state of a C# Promise can be any of the following:

- Pending
- Fulfilled
- Rejected

####Wait, But Why?
Why do I need this? C# already takes care of asynchronous programming using its async ... await statements. For Me, it's a bit frustrating to have to use different coding patterns just because i'm writing code on different platforms. One code pattern to rule them all. For asynchronous programming, JavaScript's promises rock! Other Languages already have started implementing Promises. It's only right that .NET should have it too.
