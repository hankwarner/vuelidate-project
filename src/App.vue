<template>
    <b-container>
        <b-card class="mt-3" header="Very Simple Form">
            <b-overlay :show="loading" rounded="sm">
                <b-form @submit="onSubmit">
                    <b-form-group label="Your Name:" invalid-feedback="Invalid" :state="nameState">
                        <b-form-input
                            v-model="$v.form.name.$model"
                            :class="{ 'invalid-input': $v.form.name.$error }"
                            placeholder="Enter name"
                        ></b-form-input>
                    </b-form-group>

                    <b-form-group
                        label="Email address:"
                        invalid-feedback="Invalid"
                        :state="emailState"
                    >
                        <b-form-input
                            v-model="$v.form.email.$model"
                            :class="{ 'invalid-input': $v.form.email.$error }"
                            placeholder="Enter email"
                        ></b-form-input>
                    </b-form-group>

                    <b-form-group label="Phone:" invalid-feedback="Invalid" :state="phoneState">
                        <b-form-input
                            v-model="$v.form.phone.$model"
                            :class="{ 'invalid-input': $v.form.phone.$error }"
                            placeholder="Enter phone"
                        ></b-form-input>
                    </b-form-group>

                    <b-form-group label="Food:" invalid-feedback="Invalid" :state="foodState">
                        <b-form-select
                            v-model="$v.form.food.$model"
                            :class="{ 'invalid-input': $v.form.food.$error }"
                            :options="foods"
                        ></b-form-select>
                    </b-form-group>

                    <b-form-group invalid-feedback="Invalid" :state="checkedState">
                        <b-form-checkbox-group v-model="$v.form.checked.$model">
                            <b-form-checkbox value="me"
                                >Check me out</b-form-checkbox
                            >
                            <b-form-checkbox value="that"
                                >Check that out</b-form-checkbox
                            >
                        </b-form-checkbox-group>
                    </b-form-group>

                    <b-button type="submit" variant="primary">Submit</b-button>
                </b-form>

                <!-- Upon submit, show a dismissable alert with a success/failure message that disappears after 10 secs -->
                <b-alert 
                    :variant="variant"
                    :show="dismissCountDown" 
                    dismissible
                    fade
                    @dismiss-count-down="countDownChanged"
                >
                    {{ message }}
                </b-alert>
            </b-overlay>
        </b-card>
    </b-container>
</template>

<script>
import { required, minLength, email, helpers } from "vuelidate/lib/validators";

/* Validates phone number with the following formats:
    XXX-XXX-XXXX
    XXX.XXX.XXXX
    (XXX)XXX-XXXX
    (XXX) XXX-XXXX
    XXX XXX XXXX
    XXX XXXXXXX
*/
const isPhone = helpers.regex("alpha", /^\(?([0-9]{3})\)?[-. ]?([0-9]{3})[-. ]?([0-9]{4})$/);

export default {
    name: "App",
    data() {
        return {
            form: {
                name: "",
                email: "",
                phone: "",
                food: null,
                checked: [],
            },
            foods: [
                { text: "Select One", value: null },
                "Carrots",
                "Beans",
                "Tomatoes",
                "Corn",
            ],
            loading: false,
            variant: null,
            message: "",
            dismissSecs: 10,
            dismissCountDown: 0,
        };
    },
    validations: {
        form: {
            name: {
                required,
                minLength: minLength(5)
            },
            email: {
                required,
                email
            },
            phone: {
                required,
                isPhone
            },
            food: {
                required
            },
            checked: {
                required
            }
        },
        validationGroup: ["form.name", "form.email", "form.phone", "form.food", "form.checked"]
    },
    methods: {
        onSubmit: async function (event) {
            try {
                event.preventDefault();
                this.loading = true;

                // If form is valid, flag it as successful so an alert will display the correct success/failure message
                if(!this.$v.$invalid){
                    // Mimic an API call by calling setTimeout synchronously so the loader will display for 1 second
                    await this.timeout(1000);

                    this.variant = "success";
                    this.message = "Form successfully submitted!";

                } else {
                    // Force validation so the user can see which fields are invalid
                    this.$v.$touch();

                    this.variant = "danger";
                    this.message = "Please complete all required fields.";
                }

                this.showAlert();

            } catch (e) {
                console.log(e);

            } finally {
                this.loading = false;
            }
        },
        showAlert() {
            // Starts the countdown
            this.dismissCountDown = this.dismissSecs
        },
        countDownChanged(dismissCountDown) {
            // Decrements the countdown
            this.dismissCountDown = dismissCountDown;
        },
        timeout(ms) {
            return new Promise(resolve => setTimeout(resolve, ms));
        }
    },
    computed: {
        nameState() {
            // If input field hasn't been touched by the user, don't show validation error
            return !this.$v.form.name.$error ? true : !this.$v.form.name.$invalid;
        },
        emailState() {
            return !this.$v.form.email.$error ? true : !this.$v.form.email.$invalid;
        },
        phoneState(){ 
            return !this.$v.form.phone.$error ? true : !this.$v.form.phone.$invalid;
        },
        foodState() {
            return !this.$v.form.food.$error ? true : !this.$v.form.food.$invalid;
        },
        checkedState() {
            return !this.$v.form.checked.$error ? true : !this.$v.form.checked.$invalid;
        }
    }
};
</script>

<style scoped>
    .alert {
        margin-top: 2.5vh;
    }

    .invalid-input {
        border-color: red;
    }
</style>
