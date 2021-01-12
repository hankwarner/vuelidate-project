# vuelidate-project

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

## Instructions for project
1. Add validation using for the all fields on the provided form using [Vuelidate](https://vuelidate.js.org/)
	* All fields are required, can not be black or null
	* Name field must be at least 5 characters long
	* Email field must in a valid email format
	* Use regex and a vuelidate helper to validate the phone number
2. Use the "state" binding in each form group to show/ hide the the invalid message fore ach field
3. Upon pressing the submit button, show an alert of message with a message saying whether or not the data is valid

### Rules
* Code can be added, but not removed.
* You have 48 hours from the time this project has been sent to you to complete it
* Finished project must be forked from this repo and committed to your own

### Tips
* The only markup that needs to be changed is the state binding. Otherwise, all of the work will be inside of the script tags
* There are many validators available from vuelidate out of the box. You don't necessarily need to build every validator.