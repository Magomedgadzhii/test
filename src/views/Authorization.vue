<template>
  <div class="authorization">
    <div class="authorization__form">
      <h2>LeadHit</h2>
      <p class="authorization__placeholder" v-if="visible">Введите ID</p>
      <p class="authorization__alert" v-if="alert">ID должен содержать 24 символа!</p>
      <input class="authorization__inp" @focus="focusInp" @blur="blurInp"   v-model="id"  type="text">
      <button @click="checkId" class="authorization__btn">Войти</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id:'',
      visible:true,
      alert:false
    }
  },
  methods: {
    request () {
      fetch('https://track-api.leadhit.io/client/test_auth',{
        method:'get',
        headers: {
          'api-key':'5f8475902b0be670555f1bb3:eEZn8u05G3bzRpdL7RiHCvrYAYo',
          'Leadhit-Site-id': this.id  //5f8475902b0be670555f1bb3
        }
      })
        .then (response => {
          console.log(response.ok)
          if (response.ok) {
            localStorage.setItem('leadhit-site-id', JSON.stringify(this.id));
            return response.json()
          } else {
            console.log ('error')
          }
        })
        .then (data => {
          if (localStorage.getItem('leadhit-site-id') !== null) {
            this.$router.push('/Analytics')
          } else {
            this.$router.push('/')
          }
          console.log(data)
        })

    },
    checkId () {
      if (this.id.length !== 24) {
        this.id = '';
        this.visible=false;
        this.alert = true;
      } else {
        this.request()

      }
    },
    focusInp () {
      this.visible = false;
      this.alert = false;
    },
    blurInp () {
      this.id.length > 0 ? this.visible = false:this.visible = true;

    }

  }


}
</script>

<style lang="scss" scoped>
.authorization {
  width: 100%;
  min-height: 100vh;

  background: #ccc;



  display: flex;
  align-items: center;
  justify-content: center;
  overflow-scrolling: auto;
  &__form {
    width: max-content;
    height: max-content;
    min-height: 150px;

    position: relative;

    background: #fff;

    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 20px 40px;
  }
  &__inp  {
    width:100%;
    height: 40px;

    padding-left:10px;
    font-size: 18px;


    border: 1px solid #ccc;

    outline: none;


  }
  &__btn {
    width: 100%;
    height: 40px;

    font-size: 18px;
    color: #fff;

    border: none;

    background: #000;
  }
  &__placeholder {
    position: absolute;
    top: 34%;
    left: 50px;
    color: #000;
    opacity: .4;
  }
  &__alert {
    position: absolute;
    top: 37%;
    left: 50px;
    color: red;
    font-size: 12px;

  }

}
</style>
