<template>
  <div class="comment-input-section">
    <button
      class="negative-comment-button"
      v-show="!showModalNegative"
      @click="showModalNegative = true"
    >{{ this.MainButtonText }}</button>

    <textarea
      v-show="showModalNegative"
      class="negative-comment-textarea"
      type="text"
      minlength="5"
      maxlength="1000"
      v-model="negativecomment"
    />

    <div class="buttons">
      <button
        class="small-buttons"
        type="submit"
        v-show="showModalNegative"
        @click="submitNegativeComment(), showModalNegative = false"
      >{{this.SubmitButtonText}}</button>
      <button
        class="small-buttons"
        v-show="showModalNegative"
        @click="showModalNegative = false"
      >{{ this.CloseButtonText }}</button>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase/app'
import 'firebase/firestore'
import 'firebase/firebase-functions'
export default {
     props: {
      partyDetails: { type: Object, required: true },
      MainButtonText: { type: String, required: true },
      SubmitButtonText: { type: String, required: true },
      CloseButtonText: { type: String, required: true },
    },
    data() {
        return {
        showModalNegative: false,
        negativecomment: '',
        }
    },
    methods: {
        submitNegativeComment() {
           if (this.positivecomment.length < 2) {
        this.$notify({
          message: 'No empty comment please!',
          type: 'success',
          top: true,
          closeDelay: 1500,
          hideIcon: true,
        })
      } else if (this.$store.state.user.userUID) {
        firebase
          .firestore()
          .collection(this.partyDetails.country)
          .doc(this.partyDetails.dbcode)
          .collection('negativeComments')
          .add({
            negativecomment: this.negativecomment,
            like: 0,
            likedBy: [this.$store.state.user.userUID]
          })
          .then(function(data) {
            console.log('Document successfully written!', data)
          })
          .catch(function(error) {
            console.error('Error writing document: ', error)
          })
        this.negativecomment = ''
        this.showModalNegative = false
      } else {
        console.log("👎", " You must loggin to make a comment❗️");
        this.$router.push('/')
      }
    },
    }
}
</script>

<style lang="scss" scoped>
.comment-input-section {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 1em;
}

.negative-comment-button {
  padding: 25px 50px;
  border: 0;

  font-weight: bold;
  font-family: inherit;
  cursor: pointer;
  border-radius: 10px;
  background: #ffffff;
  box-shadow: 5px 5px 19px #a6a6a6, -5px -5px 19px #ffffff;
  &:focus {
    outline: 0;
  }
  &:active {
    outline: none;
    border: none;
    box-shadow: inset 5px 5px 10px #a6a6a6, inset -5px -5px 10px #ffffff;
  }
}

.negative-comment-textarea {
  width: 100%;
  border: 1px silver solid;
  font-size: 1em;
  padding: 20px;
  font-family: inherit;
  border-radius: 10px;
  background: #ffffff;
  box-shadow: 5px 5px 19px #a6a6a6, -5px -5px 19px #ffffff;
  &:focus {
    outline: none;
    border: none;
    box-shadow: inset 5px 5px 10px #a6a6a6, inset -5px -5px 10px #ffffff;
  }
}

.small-buttons {
  margin-top: 10px;
  padding: 10px 20px;
  border: 0;
  font-weight: bold;
  font-family: inherit;
  cursor: pointer;
  border-radius: 10px;
  background: #ffffff;
  box-shadow: 5px 5px 19px #a6a6a6, -5px -5px 19px #ffffff;
  &:focus {
    outline: 0;
  }
  &:active {
    outline: none;
    border: none;
    box-shadow: inset 5px 5px 10px #a6a6a6, inset -5px -5px 10px #ffffff;
  }
}
</style>