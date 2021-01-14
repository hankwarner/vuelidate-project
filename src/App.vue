<template>
    <b-container>
        <b-card class="mt-3" header="Very Simple Form">
            <b-form @submit="onSubmit">
                <b-form-group label="Your Name:" invalid-feedback="Invalid" :state="nameState">
                    <b-form-input
                        v-model="form.name"
                        placeholder="Enter name"
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    label="Email address:"
                    invalid-feedback="Invalid"
                    :state="emailState"
                >
                    <b-form-input
                        v-model="form.email"
                        placeholder="Enter email"
                    ></b-form-input>
                </b-form-group>

                <b-form-group label="Phone:" invalid-feedback="Invalid" :state="phoneState">
                    <b-form-input
                        v-model="form.phone"
                        placeholder="Enter phone"
                    ></b-form-input>
                </b-form-group>

                <b-form-group label="Food:" invalid-feedback="Invalid" :state="foodState">
                    <b-form-select
                        v-model="form.food"
                        :options="foods"
                    ></b-form-select>
                </b-form-group>

                <b-form-group invalid-feedback="Invalid" :state="checkedState">
                    <b-form-checkbox-group v-model="form.checked">
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

            <!-- Upon submit, shows a dismissable alert with a success/failure message and disappears when dismissSecs == 0 -->
            <b-alert 
                variant="success" 
                :show="success && dismissCountDown" 
                dismissible
                fade
                @dismiss-count-down="countDownChanged"
            >
                Success!
            </b-alert>
            <b-alert 
                variant="danger" 
                :show="success == false && dismissCountDown" 
                dismissible
                fade
                @dismiss-count-down="countDownChanged"
            >
                Please complete all required fields.
            </b-alert>

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
            success: null,
            dismissSecs: 10,
            dismissCountDown: 0
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
        onSubmit: function (event) {
            event.preventDefault();

            // If form is valid, flag it as successful so an alert will display the correct success/failure message
            if(!this.$v.$invalid){
                this.success = true;
            } else {
                this.success = false;
            }

            this.showAlert();
        },
        showAlert() {
            // Starts the countdown
            this.dismissCountDown = this.dismissSecs
        },
        countDownChanged(dismissCountDown) {
            // Decrements the countdown
            this.dismissCountDown = dismissCountDown;
        },
    },
    computed: {
        nameState(){
            return !this.$v.form.name.$invalid;
        },
        emailState(){
            return !this.$v.form.email.$invalid;
        },
        phoneState(){
            return !this.$v.form.phone.$invalid;
        },
        foodState(){
            return !this.$v.form.food.$invalid;
        },
        checkedState(){
            return !this.$v.form.checked.$invalid;
        }
    }
};
</script>

<style scoped>
    .alert {
        margin-top: 2.5vh;
    }
</style>
