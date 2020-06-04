# Node.js app with passport authentication

This is an example where it is used **express** and **MongoDB** to register and authenticate users. [Here](https://nodejs-app-with-passport-auth.herokuapp.com/) is live preview

## Motivation
Recently I paid attention to Node.js and decided to work with the backend framework express. I found an online tutorial on Youtube. Also I use here MongoDB.

## Code style

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/3a2cbfceb3c04faeb38aadfa0c371ae7)](https://www.codacy.com/manual/mezgoodle/node.js-app-with-passport-authentication?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=mezgoodle/node.js-app-with-passport-authentication&amp;utm_campaign=Badge_Grade)
 
## Screenshots

![Screenshot 1](https://github.com/mezgoodle/images/blob/master/node.js-app-with-passport-authentication1.PNG)
![Screenshot 2](https://github.com/mezgoodle/images/blob/master/node.js-app-with-passport-authentication2.PNG)
![Screenshot 3](https://github.com/mezgoodle/images/blob/master/node.js-app-with-passport-authentication3.PNG)
![Screenshot 4](https://github.com/mezgoodle/images/blob/master/node.js-app-with-passport-authentication4.PNG)

## Tech/framework used

**Built with**
- [express](https://www.npmjs.com/package/express)
- [MongoDB](https://www.mongodb.com/)
- [bcryptjs](https://www.npmjs.com/package/bcryptjs)
- [passportjs](http://www.passportjs.org/)

## Dependencies

![David](https://img.shields.io/david/mezgoodle/node.js-app-with-passport-authentication)

> You can see all dependencies in `package.json` [here](https://github.com/mezgoodle/node.js-app-with-passport-authentication/network/dependencies).


## Code Example

**Login**

```js
const LocalStrategy = require("passport-local").Strategy;
const mongoose = require("mongoose");
const bcrypt = require("bcryptjs");

// Load User Model
const User = require("../models/User");

module.exports = function(passport) {
    passport.use(
        new LocalStrategy({ usernameField: "email" }, (email, password, done) => {
            // Match User
            User.findOne({ email: email })
                .then(user => {
                    if (!user) {
                        return done(null, false, { message: "That email is not registered" });
                    }

                    // Match password
                    bcrypt.compare(password, user.password, (err, isMatch) => {
                        if (err) throw err;

                        if (isMatch) {
                            return done(null, user);
                        } else {
                            return done(null, false, { message: "Password incorrect" })
                        }
                    });
                })
                .catch(err => console.log(err));
        })
    );
    passport.serializeUser((user, done) => {
        done(null, user.id);
    });

    passport.deserializeUser((id, done) => {
        User.findById(id, (err, user) => {
            done(err, user);
        });
    });

}
```

## Installation

1. Clone this repository

```bash
git clone https://github.com/mezgoodle/node.js-app-with-passport-authentication.git
```
2. Use the package manager [npm](http://www.npmjs.com/) to install dependencies.

```bash
npm install
```

3. Insert your password to database in [keys.js](https://github.com/mezgoodle/node.js-app-with-passport-authentication/blob/master/config/keys.js) like:

```js
module.exports = {
    MongoURI: "mongodb+srv://mezgoodle:<password>@nodejs-app-jkq5x.mongodb.net/test?retryWrites=true&w=majority"
}
```

## Deploy

I decided to use [GitLab CI/CD](https://docs.gitlab.com/ee/ci/) it for hosting on [Heroku](https://www.heroku.com/). [Here](https://gist.github.com/mezgoodle/4ff277a6167bf92af448f5c339a44919) you can see the example of .yml file.

## How to use?

You can use it for Sign Up/Sign In in any your application. For example, blog site

## Contribute

Let people know how they can contribute into your project. A [contributing guideline](https://github.com/zulip/zulip-electron/blob/master/CONTRIBUTING.md) will be a big plus.

## Credits

[Video](https://www.youtube.com/watch?v=6FOq4cUdH8k),
[Author](https://github.com/bradtraversy/node_passport_login)

## License

![GitHub](https://img.shields.io/github/license/mezgoodle/node.js-app-with-passport-authentication)

MIT © [mezgoodle](https://github.com/mezgoodle)
