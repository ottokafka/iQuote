
## Getting Started
1) Using the Terminal 
    git clone https://github.com/ottokafka/iQuote.git

2) Open the directory of the download and open index.html

![image](https://user-images.githubusercontent.com/21117852/39669720-b1a65032-5126-11e8-9c7d-613c27a101e0.png)

3) You will see this:

![image](https://user-images.githubusercontent.com/21117852/39669724-dd272790-5126-11e8-8127-92333c3ccea0.png)


4) Install the Web Extension Wallet. Go to the github link
 [Chrome extention wallet](https://github.com/ChengOrangeJu/WebExtensionWallet)

5) in the terminal 
git clone https://github.com/ChengOrangeJu/WebExtensionWallet.git

6) in Chrome browser: More tools / Extensions

![image](https://user-images.githubusercontent.com/21117852/39468331-0efb30e2-4d65-11e8-8d1c-f1725453ba2b.png)

click enable developer and click LOAD UNPACKED

![image](https://user-images.githubusercontent.com/21117852/39468412-6bc87ba4-4d65-11e8-8185-7e8c175a6842.png)

Select WebExtensionWallet folder and click SELECT

![image](https://user-images.githubusercontent.com/21117852/39468494-d6ecf978-4d65-11e8-8ebd-b6f0cffaf52d.png)


You will see this below

![image](https://user-images.githubusercontent.com/21117852/39468538-12119df6-4d66-11e8-98f9-e621522a0f78.png)

Open the extension and select testnet. 
Note: if you can't find the web extension on chrome it might be hidden so just click the hidden field where I icon might be. it worked for me or create a short cut key for that extension.

![image](https://user-images.githubusercontent.com/21117852/39468587-5844e68e-4d66-11e8-86f9-e225a0ccc205.png)


unlock your wallet and send NAS to your self


![image](https://user-images.githubusercontent.com/21117852/39468679-ea8da68e-4d66-11e8-96dd-3668744d97db.png)

you will see this after you sumbit nas to yourself

![image](https://user-images.githubusercontent.com/21117852/39468722-221a19e8-4d67-11e8-86d6-977814efbe15.png)

Now go back to the main Dapp
the index.html file and refresh the page

![image](https://user-images.githubusercontent.com/21117852/39669739-2630a9c0-5127-11e8-9213-a1265e94532d.png)



Below is the full Smart Contract used for this Dapp


```js
"use strict";

var MyQuote = function(text) {
    if (text) {
        var obj = JSON.parse(text);
        this.key = obj.key;
        this.value = obj.value;
    } else {
        this.key = "";
        this.value = "";
    }
};

MyQuote.prototype = {
    toString: function () {
        return JSON.stringify(this);
    }
};

var IQuote = function () {
    LocalContractStorage.defineMapProperty(this, "repo", {
        parse: function (text) {
            return new MyQuote(text);
        },
        stringify: function (o) {
            return o.toString();
        }
    });
};

IQuote.prototype = {
    init: function () {
    },

    save: function (key, value) {

        key = key.trim();
        value = value.trim();
        if (key === "" || value === ""){
            throw new Error(" You didn't enter a name or You didn't enter quote ");
        }

        var from = Blockchain.transaction.from;
        var MyQuote = this.repo.get(key);
        if (MyQuote){
            throw new Error("This name has already been used used a different name");
        }

        MyQuote = new MyQuote();
        MyQuote.author = from;
        MyQuote.key = key;
        MyQuote.value = value;

        this.repo.put(key, MyQuote);
    },

    get: function (key) {
        key = key.trim();
        if ( key === "" ) {
            throw new Error("The name is blank please enter in a name then a quote")
        }
        return this.repo.get(key);
    }
};
module.exports = IQuote;
```