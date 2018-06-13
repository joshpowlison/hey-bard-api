# Hey Bard!

Hey Bard! lets you save and pull bookmarks from a remote server for your own web serial novels, webcomics, wikis, etc. If you want the simplest integration, just use Hey Bard! with [Showpony](https://github.com/Josh-Powlison/showpony)!

## How does it work?

1. Somebody logs into a Hey Bard account at [heybard.com/account.html](https://heybard.com/account.html). Or maybe they're already logged in from their last session!

2. They visit/return to your page, where the Hey Bard API works silently behind the scenes to get and save bookmarks for them.

3. Their spot on your website is automatically saved and loaded across devices.

## Using Hey Bard

### Getting started

1. Load the following script remotely: `<script src="https://heybard.com/apis/accounts/script.js"></script>`

2. Use JavaScript to create a Hey Bard object and pass a custom id: `var heybard=new HeyBard("MyHeyBardId");`. The id is used to identify your story and grab the right bookmarks. The id can be up to 20 characters long. If your id feels too un-unique to you, add a random integer to it!

### Loading a user's account

```javascript
heybard.getAccount()
  .then(function(response){
    //If an account exists for the user
    if(response.account!==null){
      //message: a message on errors, user-messages, etc. May be blank
      if(response.message) console.log(response.message);

      //The user has a bookmark! (May be newly created)
      if(response.bookmark){
        location.href=/*Go to the bookmark*/;
      }
    }
  }).catch(function(response){
    alert("Failed call! "+response);
  });
;
```

If you use getAccount() and a bookmark hasn't been set up for the user before, one will be set up now.

### Save a bookmark

```javascript
//Save bookmarks
window.addEventListener("beforeunload",save);  //On page close
window.addEventListener("blur",save);          //On tab shift (for if users are tab moguls)

//A custom function for saving the bookmarks
function save(){
  var fileNumber=0; //Set this value however you want! You probably won't even set it in here. It's just for an example.

  //If a user's not logged in, trying to save a bookmark will throw an error. So check if they're logged in first.
  if(heybard.account) heybard.saveBookmark(fileNumber); //This returns a promise, we can check it if we want.
}
```

### Make a link to HeyBard.com

This link will bring the user back to your website after logging in to a Hey Bard account.

I recommend a direct link; no opening a new tab with `target="_blank"`. This way the user doesn't get extra tabs and you can make sure they can go to their bookmark instead of just returning to where they left off. You can even use a button to prevent middle-clicking and possible errors.

```javascript
document.getElementById("heybard-link").href=heybard.makeLink({url:"https://yourwebsite.com/"});
```

## Details

### Functions and variables

`HeyBard` objects have the following functions and variable values:

```javascript
heybard.getAccount();           //Returns async function (works like a promise)
//Contains an object with these values:
  //call: should be "get"
  //message: a message on errors, user-messages, etc
  //id: the Hey Bard Id
  //success: whether the check successfully went through (NOT whether or not they're logged in!)
  //account: the user's name, or will be null if they're not logged in

heybard.saveBookmark(int);      //Returns async function (works like a promise)
//Contains an object with these values:
  //call: should be "bookmark"
  //message: a message on errors, user-messages, etc
  //id: the Hey Bard Id
  //success: whether the check successfully went through (if they aren't logged in, this will be false)
  //account: will always be null, we don't get the user's name when saving a bookmark

heybard.makeLink({url,query});  //Returns link to heybard.com

//NOTE: If you are calling getAccount() or saveBookmark(), I recommend using the values returned from those functions- don't use the ones below. They should be the same, but you're probably a little safer using the Promise values.

heybard.id;                     //The HeyBardId you set when you created the object
heybard.account;                //The user's name (or null if no user is logged in)
heybard.lastBookmark;           //The last bookmark saved (will be null if no bookmark has been saved)
heybard.callNumber;             //The number of calls you've made to Hey Bard's servers with this object. Can be helpful to track your usage.
```

## FAQ

### What's browser support like?

Chrome, Firefox, Edge, Safari... all the major ones! Unfortunately though, I'm limited in my testing capabilities and can only check things on Chrome, Firefox, and Edge. If you run into any problems on any device, open an issue here or contact me!

### How can I support this project?

[Send me money!](https://www.paypal.me/joshpowlison) But you don't have to. If you're interested in being listed as a sponsor on the Hey Bard login page, let me know and we can talk!

### I ran into trouble.

Contact me at the support links below!

## Support

[joshuapowlison@gmail.com](mailto:joshuapowlison@gmail.com)
[Twitter](https://twitter.com/joshpowlison)
