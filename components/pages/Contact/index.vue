<template>
    <section class="p-contact">
        <modal-form :isModalActive="isModalActive" :message="message" @close="isModalActive = false" @sent="sent"></modal-form>
        <div class="p-contact__inner">
            <div class="p-contact__heading">
                <h2 class="p-contact__headingTitle">Contact</h2>
                <span class="p-contact__headingIntro">Web制作にまつわるお悩み、ご相談はお気軽にchatboxまで。</span>
            </div>
        </div>
        <div class="p-contact__ohter">
            <div class="p-contact__inner">
                <div class="p-contact__ohterHeading">
                    <h3 class="p-contact__ohterHeadingTitle">各種お問合わせ</h3>
                    <span class="p-contact__ohterHeadingIntro">各種お問い合わせをご用意しております。お気軽にご利用ください。</span>
                </div>
                <div class="p-contact__ohterContents">
                    <div class="p-contact__ohterContentsChat">
                        <span class="p-contact__ohterContentsChatTitle">SNS、チャットツールからお問合わせ</span>
                        <div class="p-contact__ohterButtons">
                            <a href="https://www.facebook.com/chatbox.inc/" target="_blank" class="p-contact__ohterButtonFacebook">
                                Facebookから<br>
                                メッセージを送る
                            </a>
                            <a href="https://chatwork.com/g/chatbox-inc" target="_blank" class="p-contact__ohterButtonTwitter">
                                ChatWorkから<br>
                                メッセージを送る
                            </a>
                        </div>
                    </div>
                    <div class="p-contact__ohterContentsTel">
                        <span class="p-contact__ohterContentsTelTitle">お電話でのお問合わせ</span>
                        <a href="tel:050-3555-1212" class="p-contact__ohterContentsTelNumber">050-3555-1212</a>
                        <span class="p-contact__ohterContentsInfo">24時間対応：メッセージ預かり</span>
                    </div>
                </div>
            </div>
        </div>
        <div class="p-contact__form">
            <div class="p-contact__inner">
                <div class="p-contact__formHeading">
                    <h3 class="p-contact__formHeadingTitle">フォームでのお問い合わせ</h3>
                    <span class="p-contact__formHeadingIntro">Webからお問い合わせ出来ます。
                        <br>以下の中からお問い合わせ内容を選択し、フォームを入力してください。
                    </span>
                </div>
                <form class="p-contact__formContact">
                    <div class="p-contact__contentWrapper">
                        <div :class="contactContentClass(item.id)" v-for="item in items" @click="changeSubject(item.id) + changeInfo(item) + inputSubject(item.title)">
                            <input type="radio" :id="item.id" :name="item.name" :value="item.id" v-model="message.subject" class="p-contact__checkBox">
                            <label :for="item.id" class="p-contact__checkBoxLabel">{{ item.title }}</label>
                        </div>
                    </div>
                    <div class="p-contact__comment">{{ info }}</div>
                    <div class="p-contact__textBoxWrapper">
                        <label for="name" class="p-contact__textBoxLabel">名前</label>
                        <div class="p-contact__textBoxArea">
                          <input type="text" id="name" name="name" class="p-contact__textBox" required v-model="message.name">
                          <p class="p-contact__textBoxError" v-if="errors.name">入力してください。</p>
                        </div>
                    </div>
                    <div class="p-contact__textBoxWrapper">
                        <label for="email" class="p-contact__textBoxLabel">E-mail</label>
                        <div class="p-contact__textBoxArea">
                          <input type="email" id="email" name="email" class="p-contact__textBox" required v-model="message.email">
                          <p class="p-contact__textBoxError" v-if="errors.email">入力してください。</p>
                          <p class="p-contact__textBoxError" v-if="errors.emailValid">正しいメールアドレス形式で入力して下さい。</p>
                        </div>
                    </div>
                    <div class="p-contact__textBoxWrapper">
                        <label for="title" class="p-contact__textBoxLabel">件名</label>
                        <div class="p-contact__textBoxArea">
                          <input type="text" id="title" name="title" class="p-contact__textBox" required v-model="message.title">
                          <p class="p-contact__textBoxError" v-if="errors.title">入力してください。</p>
                        </div>
                    </div>
                    <div class="p-contact__textAreaWrapper">
                        <label for="content" class="p-contact__textAreaLabel">内容</label>
                        <div class="p-contact__textBoxArea">
                          <textarea id="content" name="content" class="p-contact__textArea" required v-model="message.body"></textarea>
                          <p class="p-contact__textBoxError" v-if="errors.body">入力してください。</p>
                        </div>
                    </div>
                    <button type="button" class="p-contact__submit" @click="confirm($event)">送信する</button>
                </form>
            </div>
        </div>
    </section>
</template>

<script>
import ModalForm from "./ModalForm.vue";

export default {
  components: {
    ModalForm: ModalForm
  },
  data() {
    return {
      items: [
        {
          title: "Web制作",
          id: "web",
          name: "web",
          description:
            "見積り相談や制作可能かの相談からお気軽にお寄せください。"
        },
        {
          title: "技術顧問",
          id: "advise",
          name: "advise",
          description:
            "制作現場でのお困りごと、相談内容などお気軽にお寄せください。"
        },
        {
          title: "イベント",
          id: "event",
          name: "event",
          description:
            "イベント・セミナーに関するお問い合わせ、ご相談などお気軽にお寄せください。"
        },
        {
          title: "その他",
          id: "etc",
          name: "etc",
          description:
            "その他 chatbox に関するご依頼・ご相談などお気軽にお寄せください。"
        }
      ],
      message: {
        subject: "",
        title: "",
        name: "",
        email: "",
        body: ""
      },
      isModalActive: false,
      info: "",
      errors: {
        name: false,
        email: false,
        emailValid: false,
        title: false,
        body: false
      }
    };
  },
  methods: {
    contactContentClass(value) {
      return {
        "p-contact__content": true,
        "is-active": this.message.subject === value
      };
    },
    changeSubject(value) {
      this.message.subject = value;
    },
    changeInfo(item) {
      this.info = item.description;
    },
    confirm(e) {
      if (this.checkForm(e)) {
        this.isModalActive = true;
      } else {
        this.isModalActive = false;
      }
    },
    checkGenre(info) {
      if (info && info !== "お問い合わせ内容を選択してください。") return true;
    },
    checkForm: function(e) {
      if (
        this.message.name &&
        this.message.email &&
        this.message.title &&
        this.message.body &&
        this.validEmail(this.message.email) &&
        this.checkGenre(this.info)
      ) {
        this.errors.name = this.errors.email = this.errors.title = this.errors.body = this.errors.emailValid = false;
        return true;
      } else {
        if (!this.info) this.info = "お問い合わせ内容を選択してください。";

        !this.message.name
          ? (this.errors.name = true)
          : (this.errors.name = false);
        if (!this.message.email) {
          this.errors.email = true;
          this.errors.emailValid = false;
        } else {
          this.errors.email = false;
          !this.validEmail(this.message.email)
            ? (this.errors.emailValid = true)
            : (this.errors.emailValid = false);
        }
        !this.message.title
          ? (this.errors.title = true)
          : (this.errors.title = false);
        !this.message.body
          ? (this.errors.body = true)
          : (this.errors.body = false);
        e.preventDefault();
      }
    },
    validEmail: function(email) {
      let re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    },
    inputSubject(title) {
      this.message.title = title + "について";
    },
    sent() {
      this.message.subject = "";
      this.message.title = "";
      this.message.name = "";
      this.message.email = "";
      this.message.body = "";
    }
  }
};
</script>

<style lang="scss" scoped>
@import "~assets/scss/layout/_layout.scss";
@import "~assets/scss/object/component/_container.scss";
@import "~assets/scss/object/component/_title.scss";
@import "~assets/scss/object/component/_button.scss";

.p-contact {
  &__inner {
    @include c-container;
  }
  &__heading {
    margin-bottom: 50px;
    text-align: center;
  }
  &__headingTitle {
    @include c-title;
    margin-bottom: 20px;
    text-align: center;
  }
  &__headingIntro {
    letter-spacing: 4px;
    display: inline-block;
    font-size: 1.5rem;
    @include desktop {
      font-size: 1.6rem;
    }
  }
  &__ohter {
    padding: 30px 0 40px;
    background: #faf7f7;
    margin-bottom: 40px;
    @include desktop {
      padding: 50px 0 70px;
      margin-bottom: 80px;
    }
  }
  &__ohterHeading {
    text-align: center;
    margin-bottom: 15px;
    @include desktop {
      margin-bottom: 30px;
    }
  }
  &__ohterHeadingTitle {
    margin-bottom: 25px;
    font-size: 1.7rem;
    font-weight: bold;
    letter-spacing: 3px;
    @include desktop {
      margin-bottom: 30px;
      font-size: 2rem;
    }
  }
  &__ohterHeadingIntro {
    line-height: 1.8;
  }
  &__ohterButtons {
    display: flex;
    flex-wrap: wrap;
    padding: 0 20px;
    justify-content: space-between;

    @include desktop {
      flex-wrap: nowrap;
    }
  }
  &__ohterButtonFacebook {
    @include c-contactButton(facebook);
    margin-bottom: 20px;
    @include desktop {
      margin-bottom: 0;
    }
  }
  &__ohterButtonTwitter {
    @include c-contactButton(chatwork);
    margin-bottom: 40px;
    @include desktop {
      margin-bottom: 0;
    }
  }
  &__ohterContents {
    display: flex;
    flex-wrap: wrap;
    border: 1px solid #c3504f;
    padding: 40px 20px;
    box-sizing: border-box;
    background: #fff;
    @include desktop {
      flex-wrap: nowrap;
    }
  }
  &__ohterContentsChat {
    display: inline-block;
    margin-bottom: 40px;
    width: 100%;
    border-right: none;
    border-bottom: 1px solid #c3504f;
    text-align: center;

    @include desktop {
      width: 60%;
      border-right: 1px solid #c3504f;
      border-bottom: none;
      margin-bottom: 0;
    }
  }
  &__ohterContentsChatTitle {
    display: inline-block;
    margin-bottom: 3rem;
    letter-spacing: 2px;
    font-weight: bold;
    font-size: 1.3rem;
    @include desktop {
      font-size: 1.5rem;
      margin-bottom: 2rem;
    }
  }

  &__ohterContentsTel {
    display: inline-block;
    width: 100%;
    text-align: center;

    @include desktop {
      width: 40%;
    }
  }

  &__ohterContentsTelTitle {
    display: inline-block;
    margin-bottom: 2rem;
    letter-spacing: 2px;
    font-weight: bold;
    font-size: 1.3rem;
    @include desktop {
      font-size: 1.5rem;
    }
  }
  &__ohterContentsTelNumber {
    display: block;
    margin-bottom: 2rem;
    color: #c3504f;
    font-size: 3.5rem;
    line-height: 1.5;
    text-decoration: none;
  }
  &__ohterContentsTelInfo {
    display: inline-block;
  }
  &__form {
    text-align: center;
  }
  &__formHeading {
    text-align: center;
    margin-bottom: 30px;
  }
  &__formHeadingTitle {
    margin-bottom: 25px;
    font-size: 1.7rem;
    font-weight: bold;
    letter-spacing: 3px;
    @include desktop {
      margin-bottom: 30px;
      font-size: 2rem;
    }
  }
  &__formHeadingIntro {
    line-height: 1.8;
  }
  &__contentWrapper {
    max-width: 700px;
    width: 100%;
    margin: 0 auto;
  }

  &__content {
    display: inline-block;
    width: 50%;
    max-width: 47%;
    height: 50px;
    margin-bottom: 10px;
    border: 1px solid #c3504f;
    border-radius: 5px;
    color: #c3504f;
    text-align: left;
    line-height: 50px;
    transition: all 0.3s;
    cursor: pointer;
    position: relative;
    @include desktop {
      height: 60px;
      line-height: 60px;
      max-width: 270px;
      margin-bottom: 15px;
    }

    &::before {
      display: inline-block;
      content: "";
      width: 20px;
      height: 20px;
      border: 1px solid rgba(195, 80, 79, 0.5);
      background: #faf7f7;
      position: absolute;
      top: 50%;
      left: 15px;
      transform: translateY(-10px);
    }
    &::after {
      display: inline-block;
      content: "";
      width: 18px;
      height: 18px;
      background: url("/images/icon_check-mute.svg") no-repeat;
      background-size: contain;
      background-position: center;
      position: absolute;
      top: 50%;
      left: 17px;
      transform: translateY(-9px);
    }

    &:hover {
      background: #c3504f;
      color: #fff;
    }

    &:nth-child(odd) {
      margin-right: 15px;
    }
  }

  &__content.is-active {
    background: #c3504f;
    color: #fff;
    &::after {
      display: inline-block;
      content: "";
      width: 18px;
      height: 18px;
      background: url("/images/icon_check.svg") no-repeat;
      background-size: contain;
      background-position: center;
      position: absolute;
      top: 50%;
      left: 17px;
      transform: translateY(-9px);
    }
  }
  &__checkBox {
    display: none;
    margin-left: 15px;
  }

  &__checkBoxLabel {
    margin-left: 55px;
  }

  &__comment {
    width: 100%;
    height: 30px;
    color: #c3504f;
    line-height: 1.8;
    margin-bottom: 10px;
    @include desktop {
      margin-bottom: 30px;
    }
  }

  &__textBoxWrapper,
  &__textAreaWrapper {
    margin-bottom: 5px;
    text-align: left;
    @include desktop() {
      margin-bottom: 20px;
      text-align: center;
      margin-right: 80px;
    }
  }
  &__textBoxLabel,
  &__textAreaLabel {
    display: inline-block;
    min-width: 100px;
    @include desktop() {
      width: 50px;
      margin-bottom: 0;
      margin-right: 30px;
      text-align: right;
    }
  }

  &__textAreaLabel {
    vertical-align: top;
  }

  &__textBoxLabel {
    vertical-align: top;
  }

  &__textBoxArea {
    display: inline-block;
    width: 100%;
    max-width: 550px;
    @include desktop() {
      text-align: left;
    }
  }

  &__textBox {
    width: 100%;
    padding: 1.6rem;
    max-width: 550px;
    height: 30px;
    font-size: 1.5rem;
    box-sizing: border-box;
    background: #faf7f7;
    border: 1px solid #ccc;
    outline: none;
  }

  &__textBoxError {
    padding-top: 6px;
    color: #c3504f;
    font-size: 1.3rem;
  }

  &__textArea {
    border: 1px solid #ccc;
    padding: 1.6rem;
    width: 100%;
    max-width: 550px;
    height: 200px;
    font-size: 1.5rem;
    box-sizing: border-box;
    background: #faf7f7;
    resize: none;
    outline: none;
  }

  &__submit {
    display: inline-block;
    margin-bottom: 10px;
    padding: 2rem 1rem;
    width: 100%;
    text-align: center;
    border-radius: 4px;
    border: 0;
    color: #fff;
    font-size: 1.5rem;
    font-weight: bold;
    background: #c3504f;
    text-decoration: none;
    position: relative;
    transition: all 0.5s;
    outline: none;
    cursor: pointer;
    @include desktop {
      font-size: 1.5rem;
      line-height: 1.5;
      font-weight: normal;
      width: 45%;
      letter-spacing: 2px;
    }
    &:hover {
      opacity: 0.8;
      transition: all 0.5s;
      &::before {
        right: 0.2rem;
        transition: all 0.5s;
      }
    }
    &::before {
      content: "";
      display: inline-block;
      width: 1.4rem;
      height: 1.4rem;
      background-image: url("/images/icon_arrow-white.svg");
      background-repeat: no-repeat;
      background-size: contain;
      background-position: center;
      position: absolute;
      right: 0.7rem;
      top: 50%;
      transform: translateY(-50%);
      transition: all 0.5s;
      @include desktop {
        width: 2rem;
        height: 2rem;
        right: 1rem;
      }
    }
  }
}
</style>
