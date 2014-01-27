---
layout: post
title: Change (Current) User within JUnit Test Case in Google App Engine
date: 2014-02-08
categories: appengine junit java
---

### Intention
Ability to write JUnit test cases to simulate multi user scenarios. For example (fictitious  scenario):

* Bob creates BobMessage.
* Cat logs in and is able to see BobMessage.
* Dan logs in and is not able to see BobMessage.

### Problem
It is not possible to switch user within a single JUnit test case in Google App Engine (Java).

### Solution
Create a `loginAs(String, Closure)` method in Groovy to switch the injected `UserService` within Spring MVC controller object.

``` java
@Before
public void setUp() {
    super.setUp()
    new LocalServiceTestHelper(new LocalUserServiceTestConfig())
            .setEnvAttributes([
            'com.google.appengine.api.users.UserService.user_id_key': 'bob-101'
    ])
            .setEnvEmail('bob@bob.com')
            .setEnvAuthDomain('bob.com')
            .setEnvIsLoggedIn(true)
            .setUp()
}

void loginAs(String username, Closure closure) {
    UserService mockUserService = mock(UserService)
    when(mockUserService.currentUser).thenReturn(
            new User("${username}@test", 'test', "${username}-id")
    )

    UserService backupUserService = controller.userService
    controller.userService = mockUserService
    closure.call()
    controller.userService = backupUserService
}

@Test
void 'multi user test case'() {
    // default user is bob@bob.com as configured in @Before
    assertEquals('bob@bob.com', controller.userService.currentUser.email)

    // login as 'ace' and execute assertion in closure
    loginAs('ace', {
        assertEquals('ace@test', controller.userService.currentUser.email)
    })

    // login as 'cat' and execute assertion in closure
    loginAs('cat', {
        assertEquals('cat@test', controller.userService.currentUser.email)
    })

    // post loginAs method, user reverts to value set in @Before
    assertEquals('bob@bob.com', controller.userService.currentUser.email)
}
```

## Requirements
* Google App Engine (Java)
* SpringMVC (@Controller with injected UserService)
* Groovy - while this example was coded in Groovy, it is not really a requirement, you can always do similar stuff in Java.

