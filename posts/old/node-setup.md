# Resources for how to make a MERN Stack


## What is a MERN stack?

MERN (MongoDB, Express.js, React.js, Node.js) is a web development stack of entirely Javascript technologies. 
- **MongoDB**: document database
- **Express.js**: Node.js web server framework
- **React.js**: Javascript front-end framework
- **Node.js**: Javascript runtime for servers

## Format
 
Steps and resources are put in roughly chronological order of what you need to learn and the technical setup. It won't distill every bit of detail (instead you would refer to the resources), but it will point out specific microsteps/concepts.

---

## Step One: Installing Node.js and npm

*You need to install Node.js so you have local development environment to build your web app in. You also need npm to install packages.*

npm docs recommends to use a Node version manager. So instead of using the Node installer, **install [nvm-windows](https://github.com/coreybutler/nvm-windows).** The installation steps and commands are described in the README.md.


1. After installation, run Command Prompt as an admin.
2. Run `nvm install lts` to install NodeJS.
3. After running the previous command, it will output what command to run next (e.g. `nvm use 16.17.0`).

**To check version of npm and Node.js**
```
node -v
npm -v
```

---

## Step Two: Creating an Express App

*Since the MERN stack is modular by nature, you can make two separate directories for React and Express (e.g `/client`, `/server`). For Express related installations, make sure to do it in the right directory*

**Follow the Express tutorial:** [https://expressjs.com/en/starter/installing.html](https://expressjs.com/en/starter/installing.html)

The tutorial teaches you how to:
- install the Express package (remember to use `npm init`)
- how to serve requests to certain routes and static files

**You also need to set up environment variables (e.g. `PORT`)**:
1. Create a file `config.env` in your server directory.
2. `npm install dotenv`
3. Put `require("dotenv").config({ path: "./config.env" });` (default path is `.env`)
4. Access environment variables with `process.env.<variable-name>`.
---

## Step Three: Setting up MongoDB with Mongoose

*This uses MongoDB Atlas, which allows us to have a database up in the cloud instead of needing to install MongoDB locally. Mongoose is a ODM (object document mapping) that allows us to enforce schemas for our documents and uses models with methods to query the collection. It's like a ORM but for NoSQL databases.*

**Follow the MongoDB tutorial to set up the database:** [tutorial link](https://www.mongodb.com/docs/atlas/getting-started/?_ga=2.121274312.1157243813.1661114568-2020280668.1661021401&_gac=1.49282772.1661190575.Cj0KCQjw0oyYBhDGARIsAMZEuMtt4aQPsI_qMVfveAJjbOeAxaG4Xi8K8ctNEg7AKl8-0_2gxENUaEQaAqSMEALw_wcB)

Here are the basic steps to set up the database:
- make an account
- create a database/cluster (use shared tier) and put in a name
- create a database user (need it to access the collections)
- get the database connection string
- use the connection string as a environment variable

**To connect to MongoDB in the Node.js app:**
1. In `config.env`: put `MONGODB_URL=<connection-string>`.
2. Install Mongoose with `npm install mongoose`.
3. Put this in the code to connect to the database:
```javascript
mongoose = require('mongoose');
mongoose.connect(process.env.MONGODB_URL); // this is promise
```

**Extra resources:** 

- Check out NetNinja's [video](https://www.youtube.com/watch?v=s0anSjEeua8&list=PL4cUxeGkcC9iJ_KkrkBZWZRHVwnzLIoUE&index=4) on setting up MongoDB and Mongoose
- MongoDB University's [Lab](https://university.mongodb.com/mercury/M001/2022_August_23/chapter/Chapter_1_What_is_MongoDB_/lesson/5f32deb504e9ffc01ac9586c/problem) for creating a database

---

## Step Four: ReactJS

- using Create React App toolchain for single page application
    - uses webpack (bundler), eslint (analyze for any problems), babel (compiler)
    - https://create-react-app.dev/docs/getting-started
`npx create-react-app my-app --template typescript`
`npm start`
