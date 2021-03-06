# Prefill #

Prefill is a javascript library to guess what an end-user wants to look up. The guess is based upon: current location and the time.

## Why did I make this

Because a lot of journey planners exist, yet they all fail at delivering the simple user experience that a user would expect: prefill what a user will probably fill out.

## Usage ##

### General ###

We need the history in a certain format. Without further ado, this is an example of the only format we accept at this moment:

```javascript
history = [
    {
        "datetime" : "2013-08-31T02:20Z", // iso8601
        "location" : { // the location of the user when it performed the action
            "longitude" : 3.14,
            "latitude" : 51.2
        },
        "from" : "http://stations.io/exampleidentifier1",
        "to" : "http://stations.io/exampleidentifier2"
    }//,
    //...
]

```

The history array will be used as the input for prefill:

```javascript
p = new Prefill();
//this is a quite intense operation depending on how fast our neural network learns (oh yes, we're using a neural network)
p.prepare(history, function(){
    //this operation is okay
    result = p.guess("2013-08-31T02:20Z", 3.14, 51.2);
    console.log("I think " + result.to + " and " + result.from + " are the desired values");
});

```

### Give it a try using node ###

In this repository, there is a test.js. If you have node.js installed and if you performed an `npm install` in this directory, you can run the test by launching:

```bash
./test.js
```

### Use it in a browser ###

For the lazy or for those who just want to use prefill.js, you can find a ready made build in the build directory over here: https://github.com/OpenTransport/prefill/blob/master/build/bundle.js then,
* Step 1: load the script somewhere in <head>: `<script src="js/prefill.js"></script>` or whatever you renamed to bundle.js file to.
* Step 2: Create an object somewhere: `var p = new Prefill();`


For those who want to develop further and use it in the browser, sorry, you will still need node.

* Step 0: clone this repository
* Step 1: `npm install`
* Step 2: (sudo) `npm install -g browsify` (if you don't have browserify installed yet)
* Step 3 option a: `make` (there's a makefile which atm will perform option b for you)
* Step 3 option b: `browsify js/prefill.js -o build/bundle.js`
* Step 4: use it in your HTML: `<script src="build/bundle.js"></script>`
* Step 5: Create an object somewhere: `var p = new Prefill();`


## How it works ##

The history should be available on client side (e.g. in LocalStorage).

The history will be used to get all the necssary variables to train a neural network:

 * Day of the week
 * Day of the year
 * Hours and minutes
 * longitude and latitude of the users

and of course the "from" and "to" are included when training.

For the neural network, we will use [brain.js](https://github.com/harthur/brain).

### Being smarter ###

In some cases we can be smarter than the neural network.

This happens when:
 * the user's query history contains exactly 1 lookup in the near future (I suggest a timeframe of 2 hours). Because: a user may look up a certain route in the future and recheck the route when time approaches
 * _more?_

### Synchronizing the history ###

The history can be synchronized with a online user account. This is not necessary as everything can be stored in localstorage on client side. 

## License

MIT
