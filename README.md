# CSharp-.NET-Promises
If you have used promised in JavaScript, then you probably know how awesomely efficient they are making code beautiful, while still gracefully executing operations that usually take a long

This code library is my attempt at recreating JavaScript promises, which i have come to love and enjoy in Microsoft .NET

####What are Promises in Programming?
Promises are based on deference (is that a word?) which is the ability to defer or delay execution of some code, until some other code has been completed, while still leaving the processor free to do other stuff. In programming, a promise is usually a function, code block, method or code statement that you wish to execute asynchonously. You could then specify what the processor should do if the code executes successfully, or how to manipulate the result of the code, or even what should happen if an error occurs while resolving the promise.

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
  Promise.Create(() => {
    //Enter Code Here
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
    return new Promise(() => add(x, y));
  }
  
  //this could now be used as
  
  AddAsync(12345, 54321).Success((result) => {
    Console.WriteLine(result);
  });
```

####Chaining Promise Functions

The Public Methods in the Promise Class all return the Promise Object, so you can chain Methods, such as 

```
Promise.Create(() => {
  Api.Get("http://www.google.com.ng");
}).Success((htmlresult) => {
  Console.WriteLine(htmlresult);
}).Then((result) => {
  result += "<p>Hello World</p>";
}).Then((result) => {
  result += "<p>My name is Michael</p>";
}).Error((ex) => {
  Debug.WriteLine(ex.Message);
  throw ex;
}).Done(() => {
  Console.WriteLine("GET from Google.com is completed");
});
```
