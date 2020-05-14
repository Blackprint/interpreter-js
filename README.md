<p align="center"><a href="#" target="_blank" rel="noopener noreferrer"><img width="150" src="https://avatars2.githubusercontent.com/u/61224306?s=150&v=4" alt="Blackprint"></a></p>

<h1 align="center">Blackprint Interpreter for JavaScript</h1>
<p align="center">Run exported Blackprint on any JavaScript environment.</p>

<p align="center">
    <a href='https://patreon.com/stefansarya'><img src='https://img.shields.io/endpoint.svg?url=https%3A%2F%2Fshieldsio-patreon.herokuapp.com%2Fstefansarya%2Fpledges&style=for-the-badge' height='20'></a>
    <a href='https://github.com/Blackprint/Blackprint/blob/master/LICENSE'><img src='https://img.shields.io/badge/License-MIT-brightgreen.svg' height='20'></a>
</p>

This repository is designed to be used together with [Blackprint](https://github.com/Blackprint/Blackprint) as the interpreter on the Browser, Node.js, Deno, and other JavaScript environment.

## Documentation
> Warning: This project haven't reach it stable version (semantic versioning at v1.0.0)<br>

```js
// Create Blackprint Interpreter instance, `instance` in this documentation will refer to this
var instance = new Blackprint.Interpreter();
```

### Register new node interface type
An interface is designed for communicate the node handler with the JavaScript's runtime API. Because there're no HTML to be controlled, this would be little different with the browser version.

```js
interTest.registerInterface('logger', function(self, bind){
	// `bind` is used for bind `self` property with a function
	// And polyfill for ScarletsFrame element binding system

	var myLog = '...';
	bind({
		get log(){
			return myLog;
		},
		set log(val){
			myLog = val;
			console.log(val);
		}
	});

	// After that, you can get/set from `self` like a normal property
	// self.log === '...';

	// In the self object, it simillar with: https://github.com/Blackprint/Blackprint
	self.clickMe = function(){...}
});
```

## Node handler registration
This is where we register our logic with Blackprint.<br>
If you already have the browser version, you can just copy it without changes.<br>
It should be compatible if it's not accessing any Browser API.<br>

```js
sketch.registerNode('myspace/button', function(handle, node){
    // Use node handler from sketch.registerInterface('button')
    node.type = 'button';
    node.title = "My simple button";

    // Called after `.button` have been clicked
    handle.onclicked = function(ev){
        console.log("Henlo", ev);
    }
});

instance.createNode('math/multiply', {});
```

### Example
This repository provide an example with the JSON too, and you can try it with Node.js or Deno:<br>

```sh
# Change your working directory into empty folder first
$ git clone --depth 1 https://github.com/Blackprint/interpreter-js .
$ npm i
$ node ./example/init.js
```