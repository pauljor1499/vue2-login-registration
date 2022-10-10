<template>
    <div class="main-content">
        <v-card elevation="4">
            <v-card-title>Registration Form</v-card-title>
            <v-card-subtitle> Virtual Webinar Platform </v-card-subtitle>
            <v-col cols="12">
                <v-alert
                    text
                    outlined
                    type="error"
                    color="deep-orange"
                    v-show="error_message"
                >
                    Invalid account to register.
                </v-alert>
                <v-text-field
                    outlined
                    label="First name"
                    required
                    counter="10"
                    v-model="first_name"
                    :error-messages="first_nameErrors"
                    @input="$v.first_name.$touch()"
                    @blur="$v.first_name.$touch()"
                />
                <v-text-field
                    outlined
                    label="Middle name (Optional)"
                    counter="10"
                    v-model="middle_name"
                    :error-messages="middle_nameErrors"
                    @input="$v.middle_name.$touch()"
                    @blur="$v.middle_name.$touch()"
                />
                <v-text-field
                    outlined
                    label="Last name"
                    required
                    counter="10"
                    v-model="last_name"
                    :error-messages="last_nameErrors"
                    @input="$v.last_name.$touch()"
                    @blur="$v.last_name.$touch()"
                />
                <v-text-field
                    outlined
                    label="E-mail"
                    placeholder="sample@email.com"
                    required
                    counter="10"
                    v-model="email"
                    :error-messages="emailErrors"
                    @input="$v.email.$touch()"
                    @blur="$v.email.$touch()"
                />
                <v-text-field
                    outlined
                    label="Password"
                    required
                    counter="10"
                    :append-icon="show_password ? 'mdi-eye' : 'mdi-eye-off'"
                    :type="show_password ? 'text' : 'password'"
                    @click:append="show_password = !show_password"
                    v-model="password"
                    :error-messages="passwordErrors"
                    @input="$v.password.$touch()"
                    @blur="$v.password.$touch()"
                />
            </v-col>
            <v-card-actions>
                <v-btn color="green" x-large width="100%" @click="submit()">
                    Register Account
                </v-btn>
            </v-card-actions>
        </v-card>
    </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, maxLength, email } from "vuelidate/lib/validators";

export default {
    mixins: [validationMixin],

    validations: {
        first_name: { required, maxLength: maxLength(10) },
        middle_name: { maxLength: maxLength(10) },
        last_name: { required, maxLength: maxLength(10) },
        email: { required, email, maxLength: maxLength(10) },
        password: { required, maxLength: maxLength(10) },
    },

    data: () => ({
        first_name: "",
        middle_name: "",
        last_name: "",
        email: "",
        password: "",

        show_password: false,
        error_message: false,
    }),

    computed: {
        first_nameErrors() {
            const errors = [];
            if (!this.$v.first_name.$dirty) return errors;
            !this.$v.first_name.maxLength &&
                errors.push("First name must be at most 10 characters long");
            !this.$v.first_name.required &&
                errors.push("First name is required");
            return errors;
        },

        middle_nameErrors() {
            const errors = [];
            if (!this.$v.middle_name.$dirty) return errors;
            !this.$v.middle_name.maxLength &&
                errors.push("First name must be at most 10 characters long");
            return errors;
        },

        last_nameErrors() {
            const errors = [];
            if (!this.$v.last_name.$dirty) return errors;
            !this.$v.last_name.maxLength &&
                errors.push("Last name must be at most 10 characters long");
            !this.$v.last_name.required && errors.push("Last name is required");
            return errors;
        },

        emailErrors() {
            const errors = [];
            if (!this.$v.email.$dirty) return errors;
            !this.$v.email.maxLength &&
                errors.push("Email must be at most 10 characters long");
            !this.$v.email.email && errors.push("Must be valid e-mail");
            !this.$v.email.required && errors.push("E-mail is required");
            return errors;
        },

        passwordErrors() {
            const errors = [];
            if (!this.$v.password.$dirty) return errors;
            !this.$v.password.maxLength &&
                errors.push("Password must be at most 10 characters long");
            !this.$v.password.required && errors.push("Password is required");
            return errors;
        },
    },

    methods: {
        submit() {
            this.$v.$touch();

            if (this.checkAllErrors()) {
                this.error_message = false;
                console.log("Valid");
            } else {
                this.error_message = true;
                console.log("Invalid");
            }
        },

        checkAllErrors() {
            if (
                this.first_nameErrors.length == 0 &&
                this.middle_nameErrors.length == 0 &&
                this.last_nameErrors.length == 0 &&
                this.emailErrors.length == 0 &&
                this.passwordErrors.length == 0
            ) {
                return true;
            } else {
                return false;
            }
        },
    },
};
</script>

<style scoped>
.main-content {
    width: 100%;
    height: 100%;
    padding: 5%;
    display: flex;
    justify-content: center;
    align-items: center;
}

.v-card {
    width: 40%;
    padding: 50px;
}

.v-btn {
    color: white;
}
</style>
