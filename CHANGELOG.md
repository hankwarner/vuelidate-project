# Changelog

## 0.1.0 (2021-01-15)

### Added

- Form validation
    - [Vuelidate](https://vuelidate.js.org/) package added for ease of form validation.
    - All fields are required. At least one checkbox must be checked.
    - Name field must be at least 5 characters long.
    - Email and phone fields must in a valid format.
    - Invalid messages will display beneath each invalid field only after the user has interacted with the respective input field or attempted to submit the form.

- Form Submittal
    - Upon pressing the submit button, an alert is displayed with a message saying whether or not the data is valid.
    - When valid form data is submitted, a 1 second `setTimeout` is invoked synchronously in order to display the loading overlay functionality.

### Changed

- `v-models` were changed to use the form.nameuelidate model, i.e. `$v.form.name.$model` instead of `form.name`. This is required in order to use the `$error` validator when computing state changes.


## 0.0.0 (2021-01-14)

- Initial version of Vuelidate Project for assigning to candidates.
