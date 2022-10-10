<template>
    <div class="main-content">
        <v-card elevation="4">
            <v-card-title>Login Form</v-card-title>
            <v-card-subtitle> Virtual Webinar Platform </v-card-subtitle>
            <v-col cols="12">
                <v-alert
                    text
                    outlined
                    type="error"
                    color="deep-orange"
                    v-show="error_message"
                >
                    Invalid e-mail or password.
                </v-alert>
                <v-text-field
                    outlined
                    label="E-mail"
                    placeholder="sample@email.com"
                    v-model="email"
                    required
                    counter="10"
                    :error-messages="emailErrors"
                    @input="$v.email.$touch()"
                    @blur="$v.email.$touch()"
                >
                </v-text-field>
                <v-text-field
                    outlined
                    label="Password"
                    v-model="password"
                    required
                    :append-icon="show_password ? 'mdi-eye' : 'mdi-eye-off'"
                    :type="show_password ? 'text' : 'password'"
                    @click:append="show_password = !show_password"
                    :error-messages="passwordErrors"
                    @input="$v.password.$touch()"
                    @blur="$v.password.$touch()"
                >
                </v-text-field>
            </v-col>
            <v-card-actions>
                <v-btn color="green" x-large width="100%" @click="submit()">
                    Log in
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
        email: { required, email, maxLength: maxLength(10) },
        password: { required },
    },

    data: () => ({
        email: "",
        password: "",
        show_password: false,
        error_message: false,

        valid_account: {
            email: "sampleemail@gmail.com",
            password: "password2022!",
        },
    }),

    computed: {
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
            !this.$v.password.required && errors.push("Password is required");
            return errors;
        },
    },

    methods: {
        submit() {
            if (
                this.email === this.valid_account.email &&
                this.password === this.valid_account.password
            ) {
                this.error_message = false;
            } else {
                this.error_message = true;
            }
            this.$v.$touch();
        },
    },
};
</script>

<style scoped>
.main-content {
    width: 100%;
    height: 100%;
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
