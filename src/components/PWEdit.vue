<template>
  <form>
    <v-text-field
        v-model="name"
        :error-messages="nameErrors"
        :counter="100"
        label="Name"
        required
        @input="$v.name.$touch()"
        @blur="$v.name.$touch()"
    ></v-text-field>
    <v-text-field
        v-model="email"
        :error-messages="emailErrors"
        label="E-mail"
        required
        @input="$v.email.$touch()"
        @blur="$v.email.$touch()"
    ></v-text-field>
    <v-text-field
        v-model="url"
        :error-messages="urlErrors"
        label="URL"
        @input="$v.url.$touch()"
        @blur="$v.url.$touch()"
    ></v-text-field>
    <v-text-field
        v-model="password"
        :error-messages="passwordErrors"
        :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
        label="Password"
        @input="$v.password.$touch()"
        @blur="$v.password.$touch()"
        :type="showPassword ? 'text' : 'password'"
        class="mb-6"
        @click:append="showPassword = !showPassword"
    ></v-text-field>
    <v-progress-linear
        :color="score.color"
        :value="score.value"
    ></v-progress-linear>
    <v-btn id="createPassword" icon @click="createPassword">
      <v-icon>mdi-cube</v-icon>
    </v-btn>
    <label for="createPassword">文字数：{{ password_len }}</label>
    <v-row>
      <v-col
          cols="12"
          sm="4"
          md="4"
      >
        <v-checkbox
            v-model="hasAlphabet"
            label="A-Z"
            value="A-Z"
        ></v-checkbox>
      </v-col>
      <v-col
          cols="12"
          sm="4"
          md="4"
      >
        <v-checkbox
            v-model="hasNumber"
            label="0-9"
            value="0-9"
        ></v-checkbox>
      </v-col>
      <v-col
          cols="12"
          sm="4"
          md="4"
      >
        <v-checkbox
            v-model="hasSym"
            label="Sym"
            value="Sym"
        ></v-checkbox>
      </v-col>
    </v-row>
    <v-slider
        min="8"
        max=51
        v-model="password_len"
        label="Max characters"
    >
    </v-slider>
    <v-spacer></v-spacer>
    <v-btn
        class="mr-4"
        @click="submit"
    >
      登録
    </v-btn>
    <v-btn @click="clear">
      リセット
    </v-btn>
  </form>
</template>

<script>
  import zxcvbn from 'zxcvbn';
  import {validationMixin} from 'vuelidate';
  import {required, maxLength, email, url} from 'vuelidate/lib/validators';
  import accountData from "@/store/account-data.js";

  // password用文字列
  const alphabet = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
  const num = '0123456789';
  const sym = '~!@#$%^&()_+-={}[];,.';

  export default {
    name: "PWEdit.vue",
    mixins: [validationMixin],
    validations: {
      name: {required, maxLength: maxLength(10)},
      email: {required, email},
      password: {required},
      url: {url},
    },
    data: () => ({
      accountData,
      zxcvbn,
      showPassword: false,
      name: '',
      email: '',
      url: '',
      checkedTarget: false,
      password: '',
      password_len: 0,
      hasAlphabet: false,
      hasNumber: false,
      hasSym: false,
      delete_flag: false,
    }),
    computed: {
      nameErrors() {
        const errors = []
        if (!this.$v.name.$dirty) return errors
        !this.$v.name.maxLength && errors.push('Name must be at most 10 characters long')
        !this.$v.name.required && errors.push('Name is required.')
        return errors
      },
      emailErrors() {
        const errors = []
        if (!this.$v.email.$dirty) return errors
        !this.$v.email.email && errors.push('Must be valid e-mail')
        !this.$v.email.required && errors.push('E-mail is required')
        return errors
      },
      passwordErrors() {
        const errors = []
        if (!this.$v.password.$dirty) return errors
        !this.$v.password.required && errors.push('Password is required')
        return errors
      },
      urlErrors() {
        const errors = []
        if (!this.$v.url.$dirty) return errors
        !this.$v.url.url && errors.push('You need to enter the URL format.')
        return errors
      },
      score() {
        // zxcvbnで入力されたパスワードの強度をチェック
        const result = zxcvbn(this.password);

        // switch文を使って、パスワードのスコアによって、返すcolorとvalueを分岐
        switch (result.score) {
          case 4:
            return {
              color: "light-blue",
              value: 100
            };
          case 3:
            return {
              color: "light-green",
              value: 75
            };
          case 2:
            return {
              color: "yellow",
              value: 50
            };
          case 1:
            return {
              color: "orange",
              value: 25
            };
          default:
            return {
              color: "red",
              value: 0
            };
        }
      }
    },
    methods: {
      submit() {
        this.$v.$touch()
        if (!this.$v.$invalid) {
          this.addAccount();
        }
      },
      clear() {
        this.$v.$reset()
        this.name = ''
        this.email = ''
        this.password = ''
        this.url = ''
      },
      addAccount() {
        this.accountData.$data.usersAccount.push(
          {
            name: this.name,
            email: this.email,
            password: this.password,
            url: this.url,
            checkedTarget: this.checkedTarget,
            delete_flag: this.delete_flag,
          });
      },
      createPassword() {
        var str = "";
        if (this.hasAlphabet) {
          str += alphabet;
        }
        if (this.hasNumber) {
          str += num;
        }
        if (this.hasSym) {
          str += sym;
        }

        var password = '';
        for (var i = 0; i < this.password_len; i++) {
          password += str.charAt(Math.floor(Math.random() * str.length));
        }
        this.password = password;
      }
    },
  }
</script>