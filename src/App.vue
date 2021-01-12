<template>
    <b-container>
        <b-card class="mt-3" header="Very Simple Form">
            <b-form @submit="onSubmit">
                <b-form-group label="Your Name:" invalid-feedback="Invalid" :state="">
                    <b-form-input
                        v-model="form.name"
                        placeholder="Enter name"
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    label="Email address:"
                    invalid-feedback="Invalid"
                    :state=""
                >
                    <b-form-input
                        v-model="form.email"
                        placeholder="Enter email"
                    ></b-form-input>
                </b-form-group>

                <b-form-group label="Phone:" invalid-feedback="Invalid" :state="">
                    <b-form-input
                        v-model="form.phone"
                        placeholder="Enter phone"
                    ></b-form-input>
                </b-form-group>

                <b-form-group label="Food:" invalid-feedback="Invalid" :state="">
                    <b-form-select
                        v-model="form.food"
                        :options="foods"
                    ></b-form-select>
                </b-form-group>

                <b-form-group invalid-feedback="Invalid" :state="">
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
        </b-card>
    </b-container>
</template>

<script>
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
        };
    },
    validations: {},
    methods: {
        onSubmit: function (event) {
            event.preventDefault();
        },
    },
};
</script>
