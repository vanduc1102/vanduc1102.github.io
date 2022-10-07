---
layout: post
title:  Higher Order Function in Java
date:   2022-10-07 06:14:27 +0700
categories: [java]
tags: [java, core]
---

My friend just sent me a pice of code, and asked me to improve it. So it was more than 3K lines of code file. Most of the code ares duplicated.

I had found an pattern in the code:

```java

testA() {
	// preparing for the test
	while(true){
		try{
			Thread.sleep(SLEEP_FOR);
		}catch(Exception ex){

		}

		Response response = request.getA();
		// lot of parsing response
		// lot of log and assertion base on response
	}

}

```

So I thought of a solution to reduce the number line of code. What if we can use a Higher Order Function, so we can share the part of `trying with while`.

Here is my solution

```java
package com.pragmatists.blog.events.application;


import java.util.Random;
import java.util.function.Function;
import java.util.function.Supplier;


public class Main {
    public static void main(String[] args) {
        System.out.println("Hello world");
        testLikeAPro();

    }

    private static void testLikeAPro(){
        String searchName = "toro dep trai 10-11";
        Supplier<Response> lazySearchName = () -> {
            SearchApi searchAPI = new SearchApi();
            return searchAPI.searchConKet(searchName);
        };

        Response response = requestWithTry().apply(lazySearchName);
        System.out.println("status: "+ response.status + "; data:"+response.data);
    }

    private  static Function<Supplier<Response>, Response> requestWithTry(){
        return (request) ->{
            int count = 0;
            while(true){
                System.out.println("tried:  "+ ++count);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
                Response response =  request.get();
                if(response.status == 200){
                    return response;
                }
            }

        };
    }

    static class RequestAPI{
        Response get(String url){
            int[] statuses = new int[]{200, 400, 500};
            Random random = new Random();
            return new Response(statuses[random.nextInt(statuses.length)], "response of: " + url);
        }
    }

    static class SearchApi extends  RequestAPI {
        Response searchConKet(String name) {
            System.out.println("Search conket: "+ name);
            return get("?conket=where&name="+name);
        }
    }

    static class Response {
        int status;
        String data;

        public Response(int status, String data) {
            this.status = status;
            this.data = data;
        }
    }

}

```

### References:

1. [https://medium.com/@knoldus/functional-java-lets-understand-the-higher-order-function-1a4d4e4f02af](https://medium.com/@knoldus/functional-java-lets-understand-the-higher-order-function-1a4d4e4f02af)

1. [https://www.baeldung.com/java-8-functional-interfaces](https://www.baeldung.com/java-8-functional-interfaces)
