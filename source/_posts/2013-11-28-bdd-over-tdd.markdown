---
layout: post
title: "BDD over TDD"
date: 2013-11-28 01:02
comments: true
categories: nodejs, bdd
---

Today i'm going to talk a little bit about unit-tests. First rule of any programmer - "no tests, no product". Unfortunately, i got used to it too late)) Of course there are always some exceptions to that rule, but in 95% of today tasks it is mandatory. 

Best unit-tests in my opionion are:

1. Written by "non-techincal" people, like your Client, your Tester, your Grandma. 
  *Yes, i can tell integration test from unit-test. The statement above is still correct for UNIT-tests too!*
2. Written before coding is done. 
3. Small. 
4. Fast: Asynchronous. Have no side-effects. 
5. Easy-to write and to read.

Since i have been using Node.js and decided to implement my real-world backend server using this techology, i really loved BDD style-tests.
BDD is a little different from TDD, you can read about it somewhere else ~~over the rainbow~~ in the internet. But mostly i like it because test descriptions are self-explanatory.

As an example...here is the result of running **wovs** testing-tool for **forever** library:

```
♢ forever/core/tail 

When using forever the tail() method
     ✓ should respond with logs for the script
When the tests are over stop all forever processes
     ✓ should stop the correct number of procs

♢ forever/service/simple 

When using forever the service module
     ✓ should have the correct exports

♢ forever/workers/multiple 

When using forever and spawning two processes using the same script
     ✓ should respond with no error
When using forever and spawning two processes using the same script requests against the second child
     ✓ should respond with i know nodejitsu
     ✓ stop the child process
When using forever and spawning two processes using the same script requests against the first child
     ✓ should respond with i know nodejitsu
     ✓ stop the child process
Once the stop attempt has been made
     ✓ the processes should be dead
...
```

Do you see these small checks? They mean all tests have passed. How cool that we have human-readable descriptions!
Next is few lines of **mocha** output for my backend server:
```
Code processing module
     removeWhitespaces
          ✓ should remove whitespaces 
     substituteCode
          ✓ should change code 
     isJsonOk
          ✓ should check received JSON 
          ✓ should check for error 
          ✓ should handle empty err and throw 
     changeTransactionId
          ✓ should throw error if Request-New not found 
          ✓ should change ID 
     makeResponseJson
          ✓ should fill all fields 
          ✓ should throw if no Request-New found 
          ✓ should throw if empty string is passed 
          ✓ should throw if same subst is found 
     makeId
          ✓ should make ID of required length 
     ...
```

Tests are written like this:
```js 

describe('Code processing module',function(){
     describe('removeWhitespaces',function(){
          it('should remove whitespaces',function(){
               var before = 'hello world';
               var mustBe = 'helloworld';

               var out = myLib.removeWhitespaces(before); 
               assert.equal(mustBe,out);
          })
     })

     describe('substituteCode',function(){
          it('should change code',function(done){
               var template = 'json/code_request.json.utf8';

               fs.readFile(template, 'utf8', function (err, data) {
                    var changeTo = 'AAAAAAABBBBBBBC';

                    assert.equal(15,changeTo.length);
                    var index = data.indexOf(changeTo);
                    assert.equal(-1,index);

                    var out = myLib.substituteCode(data,changeTo); 
                    var index = out.indexOf(changeTo);
                    assert.notEqual(-1,index);

                    done();
               })
          })

          it('should do something cool...',function(){
               // TODO: do something cool
               // TODO: assert that it is really cool!
          })

          ...
     })

     ...
})
```

I really recommend **mocha** framework for Node.js developement. Simpy run it like this:
```bash
mocha --reporter spec 
```
I played with **vows**, **expresso** and **mocha**.
Mocha is really the best of all (i pronounce it like 'mokka').

Remember: each test saves one whale. Cheers!



