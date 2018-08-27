# HandlerInterceptor Demo
This small app demonstrates an unexpected behaviour when using a [`HandlerInterceptor`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerInterceptor.html) with Spring MVC and Spring 5.

The app provides one single REST endpoint under the path `/hello`. For demonstration purposes a very simple `HandlerInterceptor` is registered.
This interceptor roughly outlines a common implementation of the `preHandle` method:
The `handler` is casted down to a [`HandlerMethod`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/method/HandlerMethod.html) in order to access information like the method annotations etc.

This implementation worked fine with Spring 4.x. With Spring 5 however an exception is thrown when a non-existing endpoint is called (expected behaviour would be a 404 Not Found).

## How To Test the behaviour
The following command results in a 200 OK and in a log message from within the interceptor.

    curl localhost:8080/hello
    
The following command however does not show the expected behaviour (which would be a 404):
    
    curl localhost:8080/foo
    
Instead, a 500 is returned with the following message:

    org.springframework.web.servlet.resource.ResourceHttpRequestHandler cannot be cast to org.springframework.web.method.HandlerMethod
