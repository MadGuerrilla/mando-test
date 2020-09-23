<template>
<div class="l-default">
  <div class="o-user">
    <div class="c-user__avatar"></div>
    <div class="c-user__details">
      <div class="c-user__details-info">
        <h1 class="c-user__details-info-name">{{userData.name}}</h1>
        <h2 class="c-user__details-info-username">(Your Username: {{userData.username}})</h2>
      </div>
      <p class="c-user__details-bio">
        {{userData.bio}}
      </p>

      <div class="c-user__details-progress">
        <div class="c-user__details-progress-bar">
          <div class="c-user__details-progress-bar-value" :style="{maxWidth: completion() + '%'}" :data-value="completion ()"></div>
        </div>
        <h2 class="c-user__details-progress-completion">Completed {{completion () }}&#37; of modules</h2>
        <ul class="c-user__details-progress-list">
          <li class="c-user__details-progress-list-item">
            {{modulesCalc ('completed') }} Modules Completed
          </li>
          <li class="c-user__details-progress-list-item">
            {{modulesCalc ('progress') }} Modules Progress
          </li>
          <li class="c-user__details-progress-list-item">
            {{modulesCalc ('unstarted') }} Modules Remaining
          </li>
        </ul>
      </div>
    </div>
  </div>

  {{userData}}
</div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'app',
  data() {
    return {
      userData: null,
    }
  },
  mounted() {
    this.getUserData()
  },
  methods: {
    getUserData: function() {
      axios
        .get('/static/data.json')
        .then(response => this.userData = response.data)
    },
    modulesCalc: function(type) {
      let count = 0
      this.userData.courses.filter((val) => {
        if (val.status === type) count++
      })
      return count
    },
    completion: function() {
      const courseCount = this.userData.courses.length
      const completed = this.modulesCalc('completed')
      return (completed / courseCount * 100)
    }
  }
}
</script>

<style lang="scss" scoped>
$green: #aadb1e;
$grey: #ccc;
.l {
    &-default {
        font-family: Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #565656;
        max-width: 1200px;
        width: 100%;
        margin: 24px auto;
    }
}

.o {
    &-user {
        border-radius: 4px;
        border: 1px solid $grey;
        width: 100%;
        padding: 16px;
        min-height: 196px;
        display: flex;
    }
}

.c {
    &-user {
        &__avatar,
        &__details {
            width: 100%;
        }
        &__avatar {
            padding: 32px;
            @media only screen and (min-width: 992px) {
                max-width: 10%;
                background: url("./assets/svgs/avatar.svg") no-repeat top center;
                background-size: contain;
            }
        }
        &__details {
            @media only screen and (min-width: 992px) {
                max-width: 60%;
            }
            padding: 32px;
            &-info {
                display: flex;
                align-items: baseline;

                &-name,
                &-username {
                    display: inline;
                }
                &-name {
                    font-size: 2rem;
                }
                &-username {
                    font-size: 1rem;
                    padding-left: 8px;
                }
            }
            &-progress {

                &-bar {
                    border-radius: 5rem;
                    border: 1px solid $grey;
                    padding: 8px;
                    margin: 32px 0;
                    &-value {
                        width: 100%;
                        height: 16px;
                        background-color: $green;
                        border-radius: 5rem;
                        overflow: hidden;
                        background-color: $green!important;
                    }
                }

                &-list {
                    list-style: none;
                    padding: 0;
                    margin: 0;
                    &-item {
                        font-weight: bold;
                        margin-bottom: 8px;
                    }
                }
            }
        }
    }
}
</style>
