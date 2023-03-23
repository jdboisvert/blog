# Account Balance Masking - Because Why Not Have More Privacy?

No one likes it when someone is peeping over your shoulder. This is even more the case when you're logged into your personal banking with all your balance details on display. This is why I decided to build the [Account Balance Masker](https://github.com/jdboisvert/account-balance-mask) Chrome Extension. 

I was quite surprised this was not something some financial sites offered out of the box. My goal with this app was to be able to set it and forget it and not have to worry about doing a lot of set up in order for me to just mask my account balances. Also being a developer I was curious to see what it took to make a chrome extension myself.

![A gif displaying how hovering over the masked values shows the original value](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vcutj48u673quzxngf3c.gif)

### But how does it work? 
In the spirit of open source software the [code](https://github.com/jdboisvert/account-balance-mask/tree/main) is available to review if you wish to dig into it more but this is a basic overview. 

All Chrome extensions are defined by a `manifest.json` which contains information that defines it and my extension is no different. Within this extension we make use of the key `content_scripts`. [Content scripts](https://developer.chrome.com/docs/extensions/mv3/content_scripts/) is how we are able to run javascript files against specific webpages and ensure it only runs in these defined pages. With the help of wildcards this makes it easy to, for example, specify scripts to only one for 1 banking webpage. 

Within these javascript files one simply needs to search the DOM and update the elements containing the account balance details. As you can imagine this can vary from site to site but making use of `document.querySelectorAll` we are able to search through elements using wildcards as well and test if the text contains digits. With the help of the browser's developer tools one can easily find the HTML tag's class and/or id of the element and update the script as needed. 

Since this is only loaded once the page loads or whenever navigating around the webpage there is no need to store any data except within the script. You can also always view the details since the initial value is within a [closure](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures). 

### Final thoughts 
I hope others will find the simple extension useful and I hope to implement other features and webpages to help your get more privacy from those people wanting to peep at your screen. 

#### Update
Want to give it a try? You can download it on [The Chrome Web Store](https://chrome.google.com/webstore/detail/account-balance-masker/hkmponiffpfpnjbgddecfhneaajddfff?hl=en&authuser=2) now!

Happy coding!



