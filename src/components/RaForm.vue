<template>
    <div class="form-container">
      <div style="text-align: center;">
        <h1 style="margin-bottom: 0px;">Спасибо за установку!</h1>
        <p style="color: #17505B;">Остался последний шаг</p>
      </div>

      <div style="text-align: center; background-color: #BACBCE; color: #303133; padding: 15px 0; border-radius: 5px 5px 0 0;"><h2 style="margin: 0;">Активация тестового доступа</h2></div>
      <div class="activation-form">
        <p style="color: #303133; font-size: medium;">Для активации 14-дневного тестового доступа заполните форму ниже.</p>
        <div style="background-color: #E8EEEF; color: #17505B; padding: 10px; margin-bottom: 20px; border-radius: 5px; font-size: small;">
          <p style="margin: 0;"><b>Внимание!</b> В целях безопасности обслуживание будет осуществляться только по указанному номеру телефона.</p>
        </div>

        <div style="display: flex; flex-direction: column;">
          <div class="activation-form__section">
            <RaSelect :items="countries" placeholder="+7" :value="countries[0]" width="100px"></RaSelect>
            <RaInput
              :filled="false"
              :error="false"
              :disabled="false"
              placeholder="(999) 123-45-67"
              :value="maskedPhone"
            />
          </div>
          <div class="activation-form__section">
            <RaInput :value="role" :filled="false" :error="roleError" :disabled="false" placeholder="Как к вам обращаться?*"></RaInput>
          </div>

          <div class="activation-form__section">
            <RaSelect :items="roles" placeholder="Ваша роль в компании?*"></RaSelect>
          </div>

          <div class="activation-form__section">
            <label class="custom-checkbox">
              <input type="checkbox" v-model="helpCheckbox">
              <span class="checkmark"></span>
              Хочу получть бесплатную помощь в настройке
            </label>
          </div>
          <div class="activation-form__section">
            <label class="custom-checkbox">
              <input type="checkbox" v-model="promoCodeCheckbox">
              <span class="checkmark"></span>
              У меня есть промокод
            </label>

            <div v-if="promoCodeCheckbox" class="promo-code-input">
              <RaInput :value="promoCode" :filled="false" :error="false" :disabled="false" placeholder="Введите промокод"></RaInput>
            </div>
          </div>

          <div style="display: flex; margin-top: 30px;">
            <RaButton @click="activateAccess">Активировать доступ</RaButton>
          </div>
        </div>

      </div>

      <div style="display: flex; justify-content: center; font-size: small; margin-top: 15px;">
        <span>Есть вопросы? <a href="#"> Напишите нам!</a></span>
      </div>

    </div>
</template>

<script>
import RaButton from './RaButton.vue'
import RaInput from './RaInput.vue'
import RaSelect from './RaSelect.vue'

  export default {
    name: 'ActivationForm',
    components: {
      RaButton,
      RaInput,
      RaSelect,
    },
    data() {
      return {
        selectedCountry: null,
        selectedRole: null,
        helpCheckbox: true,
        promoCodeCheckbox: false,
        promoCode: '',
        maskedPhone: '',
        role: '',
        roleError: false,
        isOpen: {
          country: false,
          role: false,
        },
        countries: [
          '+7',
          '+1',
          '+3',
          '+5',
          '+12',
        ],
        roles: [
          'Собственник',
          'Руководитель отдела продаж',
          'Технический специалист',
          'Интергратор amoCRM'
        ]
      };
    },
    methods: {
      toggleDropdown(dropdown) {
        for (let key in this.isOpen) {
          if (key !== dropdown) {
            this.isOpen[key] = false;
          }
        }
        this.$set(this.isOpen, dropdown, !this.isOpen[dropdown]);
      },
      selectOption(option, type) {
        if (type === 'country') {
          this.selectedCountry = option.name;
        } else if (type === 'role') {
          this.selectedRole = option;
        }
        this.isOpen[type] = false;
      },
      handleClickOutside(event) {
        if (this.isOpen.country && !this.$refs.countryDropdown.contains(event.target)) {
          this.isOpen.country = false;
        }
        if (this.isOpen.role && !this.$refs.roleDropdown.contains(event.target)) {
          this.isOpen.role = false;
        }
      },
      applyPhoneMask(value) {
        let numbers = value.replace(/[^\d]/g, '');
        let char = {0:'(',3:') ',6:'-',8:'-'};
        let masked = '';

        for (let i = 0; i < numbers.length; i++) {
          masked += (char[i] || '') + numbers[i];
        }

        this.maskedPhone = masked;
      },
      activateAccess() {
      },
    },
    mounted() {
      document.addEventListener('click', this.handleClickOutside);
    },
    beforeDestroy() {
      document.removeEventListener('click', this.handleClickOutside);
    }
  };
  </script>

<style scoped lang="scss">
.form-container {
  max-width: 500px;
  margin: 0 auto;
}

.activation-form {
  padding: 20px;
  border-radius: 0 0 4px 4px;
  border: 1px solid #BDBDBD;
}

.activation-form h2 {
  color: #333;
}

.activation-form p {
  color: #666;
}

.activation-form__section {
  display: flex;
  flex-direction: row;
  align-items: center;
  min-height: 40px;
  margin-bottom: 10px;
}

.promo-code-input {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-left: 15px;
}

a {
  text-align: center;
  color: #17505B;
  text-decoration: none;
}

label {
  color: #17505B;
  font-weight: 600;
  font-size: small;
}

.custom-checkbox {
  display: inline-block;
  position: relative;
  padding-left: 25px;
  cursor: pointer;
  user-select: none;
}

/* Hide the browser's default checkbox */
.custom-checkbox input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom checkmark */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 14px;
  width: 14px;
  background-color: #fff;
  border: 1px solid #495057;
  border-radius: 4px;
}

/* On mouse-over, add a grey background color */
.custom-checkbox:hover input ~ .checkmark {
  background-color: #fff;
}

/* When the checkbox is checked */
.custom-checkbox input:checked ~ .checkmark {
  background-color: #17505B;
}

/* Create the checkmark/indicator (hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the checkmark when checked */
.custom-checkbox input:checked ~ .checkmark:after {
  display: block;
}

/* Style the checkmark/indicator */
.custom-checkbox .checkmark:after {
  left: 5px;
  top: 1px;
  width: 4px;
  height: 8px;
  border: solid white;
  border-width: 0 1px 1px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}
</style>