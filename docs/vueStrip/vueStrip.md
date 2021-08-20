# Vue 中使用 Strip 来验证信用卡

## 安装包依赖

```javascript
npm i vue-stripe-elements-plus --save
```

## 在开始的 index.html 引入 v3

```javascript
<script src="https://js.stripe.com/v3/"></script>
```

## 重点两个参数

> stripe 必须要你自己设定 options 必须要有参数,可以参考下面的链接

- 链接如下

> https://stripe.com/docs/stripe.js#element-options

## 验证信用卡号,cvc,过期日期

> 验证 Number,cvc,过期时间

### 完整版

```html
<template>
  <div class="credit-card-inputs" :class="{ complete }">
    <card-number
      class="stripe-element card-number"
      ref="cardNumber"
      :stripe="stripe"
      :options="options"
      @change="number = $event.complete"
    />
    <card-expiry
      class="stripe-element card-expiry"
      ref="cardExpiry"
      :stripe="stripe"
      :options="options"
      @change="expiry = $event.complete"
    />
    <card-cvc
      class="stripe-element card-cvc"
      ref="cardCvc"
      :stripe="stripe"
      :options="options"
      @change="cvc = $event.complete"
    />
  </div>
</template>

<script>
  import { CardNumber, CardExpiry, CardCvc } from 'vue-stripe-elements-plus';

  export default {
    props: ['stripe', 'options'],
    data() {
      return {
        complete: false,
        number: false,
        expiry: false,
        cvc: false,
      };
    },
    components: { CardNumber, CardExpiry, CardCvc },
    methods: {
      update() {
        this.complete = this.number && this.expiry && this.cvc;

        // field completed, find field to focus next
        if (this.number) {
          if (!this.expiry) {
            this.$refs.cardExpiry.focus();
          } else if (!this.cvc) {
            this.$refs.cardCvc.focus();
          }
        } else if (this.expiry) {
          if (!this.cvc) {
            this.$refs.cardCvc.focus();
          } else if (!this.number) {
            this.$refs.cardNumber.focus();
          }
        }
        // no focus magic for the CVC field as it gets complete with three
        // numbers, but can also have four
      },
    },
    watch: {
      number() {
        this.update();
      },
      expiry() {
        this.update();
      },
      cvc() {
        this.update();
      },
    },
  };
</script>

<style>
  .credit-card-inputs.complete {
    border: 2px solid green;
  }
</style>
```

## 支持的方法

- createToken()

> https://stripe.com/docs/js#stripe-create-token

- createSource()

> https://stripe.com/docs/js/tokens_sources/create_source

- retrieveSource()

> https://stripe.com/docs/js/tokens_sources/retrieve_source

- paymentRequest()

> https://stripe.com/docs/js/payment_request/create

- redirectToCheckout()

> https://stripe.com/docs/js/checkout/redirect_to_checkout

- retrievePaymentIntent()

> https://stripe.com/docs/js/payment_intents/retrieve_payment_intent

- handleCardPayment()

> https://stripe.com/docs/js/deprecated/handle_card_payment_element

- handleCardSetup()

> https://stripe.com/docs/js/deprecated/handle_card_setup_element

- handleCardAction()

> https://stripe.com/docs/js/payment_intents/handle_card_action

- confirmPaymentIntent()

> https://stripe.com/docs/js/deprecated/confirm_payment_intent_element

- createPaymentMethod()

> https://stripe.com/docs/js#stripe-create-payment-method
