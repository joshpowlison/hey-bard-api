# Hey Bard!

Hey Bard! is a system that lets you save and pull bookmarks from a remote server for your own web serial novels, webcomics, etc.

This page is still undergoing changes, because I'm working on making the most user-friendly setup I can! The files aren't even live yet. That said, if you have any questions or are interested now, let me know! I think it will be be public by the end of March 2018.

Hey Bard! integration is built into [Showpony](https://github.com/Josh-Powlison/showpony) for easy account use, but can be set up for your own website too with a little work!

## How does it work?

When a user logs into a Hey Bard account on heybard.com, they can call files from heybard.com from other websites. As long as it's called through the user's browser (and not PHP), the user can get their data. So without touching passwords or delicate user data, you can call files from heybard.com to get the user's bookmark info!

## Using Hey Bard

### Getting started

Start by calling this file: `undetermined`

Once you've loaded it, you can create a Hey Bard object in JavaScript and pass a custom id:

`var heybard=new HeyBard("MyHeyBardId"); //The id allows Hey Bard to identify your story`

You can make the id whatever you want! If your id seems too un-unique add a random integer at the end of it. Hey Bard Ids can be up to 20 characters long.

`HeyBard` objects have just 3 functions. Each returns a JavaScript promise:

```
heybard.getAccount();           //Returns async function
heybard.saveBookmark(int);      //Returns async function
heybard.makeLink({url,query});  //Returns link to heybard.com
```

### Loading a user's account and bookmark/creating a new bookmark

The `getAccount()` function returns an async function, which works like a promise. Here's a sample of the code.

```
//Save the user's account name
var account=null;

heybard.getAccount()
  .then(function(response){
    //returns the following JSON object:
      //call: will be "get" for getAccount
      //message: a message on errors, user-messages, etc
      //id: the Hey Bard Id
      //success: whether the check successfully went through (NOT whether or not they're logged in!)
      //account: the user's name, or will be null if nonexistent
  
    //If an account exists for the user
    if(response.account!==null){
      //Save the user's account name as a variable
      account=response.account;
    
      //message: a message on errors, user-messages, etc
      if(response.message) console.log(response.message);

      //If you have a spot to put the user's name, you can put it here!
      document.getElementById("username").innerHTML=response.account;

      //The user has a bookmark! (if the account didn't have an associated bookmark before, it does now!)
      if(response.bookmark){
        /*****
          Send the user to the right page
            If your website has an easy numbering system, you can just send them to the webpage simply!
            If not, here are some options:
            1. Pass an array of file names to the user. Go to the URL specified at the bookmark's spot in the array.
            2. Fetch a custom PHP file with a promise. That file can return the right name for the file.
        *****/

        location.href=webpageToGoTo;
      }
    }
  }).catch(function(response){
    alert("Failed call! "+response);
  });
;
```

### Save a bookmark

You'll generally want to save bookmarks in two different cases: when the tab is closed, and when the user moves to another tab. The following code will save the user's spot, you just have to update the `fileNumber` value consistently.

```
//Save bookmarks
window.addEventListener("beforeunload",saveHBBookmark);  //On page close
window.addEventListener("blur",saveHBBookmark);          //On tab shift (for if users are tab moguls)

//A function for saving the bookmarks
saveHBBookmark(){
  var fileNumber=0; //Set this value however you want! On page load, by checking the URL...

  //ONLY SAVE A BOOKMARK IF THE USER'S LOGGED IN ON THIS PAGE!!!! Because of the way the calls work, if the user logs in and then returns to this page in a wrong order, their bookmark can easily be overwritten. You need to make sure that they were logged in when the page loaded. Account was set above in the getAccount() example.
  if(account) saveBookmark(fileNumber);
}
```

### Make a link to HeyBard.com

This link will bring the user back to your website after logging in to a Hey Bard account.

I recommend a direct link; no opening a new tab with `target="_blank"`. This way the user doesn't get extra tabs and you can make sure they can go to their bookmark instead of just returning to where they left off.

```
//Make a link to heybard.com
document.getElementById("heybard-link").href=heybard.makeLink({url:"https://yourwebsite.com/"});
```

## FAQ

I don't have any questions yet because this is so new! Feel free to [@ me on Twitter though](https://twitter.com/joshpowlison) to ask me anything!
