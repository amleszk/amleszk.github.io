---
layout: post
title: "unit testing block based callbacks"
date: 2013-10-19 20:11
comments: true
categories: 
- iOS 
- unit testing

---

I recently tackled unit testing my AFNetworking client classes, which use block based callbacks, stubbing out block callbacks took some trial and error so is worth of a blogpost. 

Take the example method:

```
- (AFHTTPRequestOperation*)meWithSuccess:(void (^)(AFHTTPRequestOperation *operation, NSDictionary *responseObject))success
                                 failure:(void (^)(AFHTTPRequestOperation *operation, NSError *error))failure

```

How would you go about mocking/stubbing this method call? One way is to use [OCMock expectations](http://ocmock.org/). In your unit test setUp:

```
self.stubMessagesPayload = @{
     @"has_mail" : @0,
     @"has_mod_mail" : @0,
};

_client = [OCMockObject mockForClass:[AFRedditAPIClient class]];

[[[_client expect] andDo:^(NSInvocation *invocation) {
    void (^__unsafe_unretained successStub)(AFHTTPRequestOperation *, NSDictionary *);
    [invocation getArgument:(&successStub) atIndex:2];
    
    successStub(nil,self.stubMessagesPayload);
}] meWithSuccess:[OCMArg any] failure:[OCMArg any]];    

```

In the actual test, you can change the value of `stubMessagesPayload` and assert different behaviour. You'll also notice the block argument is located at index 2, [from the documentation](https://developer.apple.com/library/mac/documentation/cocoa/reference/foundation/classes/NSInvocation_Class/Reference/Reference.html#//apple_ref/occ/instm/NSInvocation/getArgument:atIndex:):

`Indices 0 and 1 indicate the hidden arguments self and _cmd, respectively; these values can be retrieved directly with the target and selector methods. Use indices 2 and greater for the arguments normally passed in a message.`

The other issue you may face is how to get your class to actually use the mocked version of your object. The code under test may look like this:

```
    [[AFRedditAPIClient sharedClient] meWithSuccess:^(AFHTTPRequestOperation *operation, NSDictionary *responseObject) {
    NSLog(@"success");
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
    NSLog(@"failure");
    }];

```

The only way to stub a class method is via swizzling, which can get messy. The alternative is dependency injection, whereby you expose object dependencies using properties or constructor varaibles. Then they can be swapped more easily at runtime, so if the `AFRedditAPIClient` object where a public property, and `RCMessageClient` were the class i wanted to test, expose the dependency using a property and in the test asign it to the mock object:

```
_apiClient = [OCMockObject mockForClass:[AFRedditAPIClient class]];
_messageClient = [[RCMessageClient alloc] init];
_messageClient.apiClient = _apiClient
...
Additional stubs explained above
...
```

And the implementation changes to refer to that property instead of the class method:

```
    [_apiClient meWithSuccess:^(AFHTTPRequestOperation *operation, NSDictionary *responseObject) {
    NSLog(@"success");
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
    NSLog(@"failure");
    }];

```