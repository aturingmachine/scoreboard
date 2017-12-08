<template>
  <v-app>

    <v-toolbar fixed app :clipped-left="clipped">
      <v-toolbar-title v-text="title"></v-toolbar-title>

      <v-dialog v-model="gameOver" lazy absolute width="50%">
        <span class="display-4">
      <span v-if="winner == 1"> Congrats {{ p1name }} </span>
      <span v-else> Congrats {{ p2name }} </span>
        </span>

      </v-dialog>

      <v-dialog v-model="editDialog" lazy absolute width="50%">
                  <v-btn slot="activator" class="green lighten-1 white--text">
                    New Game
                  </v-btn>
                  <v-card>
                    <v-toolbar>
                        <div> Edit Players </div>
                      </v-toolbar>
                    <v-container fluid>
                      <v-card-text>

                        <v-form ref="form">
                          <v-text-field v-model="p1name" label="Player 1"></v-text-field>
                          <v-text-field v-model="p2name" label="Player 2"></v-text-field>
                          
                          <v-checkbox label="Is Player 1 Serving?" v-model="p1serving"></v-checkbox>


                          <v-btn @click="editDialog = false" class="red white--text">Close</v-btn>
                        </v-form>
                      </v-card-text>
                    </v-container>
                  </v-card>
</v-dialog>
<v-spacer></v-spacer>
<v-btn @click="clear()" class="red white--text">Clear</v-btn>
    </v-toolbar>




    <v-content>
      <v-container fluid>
        <v-card>

          <!-- p1 -->
          <v-layout row-wrap>
          <v-flex xs-6>
          <v-card class="blue lighten-5">
            <v-toolbar class="blue">
              <v-toolbar-title class="white--text">
                 {{ p1name }} <span v-if="p1serving"> <v-icon class="white--text">radio_button_checked</v-icon></span> 
              </v-toolbar-title>
            </v-toolbar>
            <v-flex xs-4></v-flex>
            <span class="display-4 text-xs-center"> <span class="blue--text"><strong>{{score1}}</strong></span>
              <br>
              <v-btn @click="score1 -= 1" class="red white--text">DECEREMENT</v-btn>
              <v-btn @click="addPoints(1)" class="green white--text">INCEREMENT</v-btn>
              
            </span>
            <v-flex xs-4></v-flex>
          </v-card>
          </v-flex>

          <!-- p2 -->
          <v-flex xs-6>
          <v-card class="red lighten-5">
            <v-toolbar class="red">
              <v-toolbar-title class="white--text">
                {{ p2name }}<span v-if="p1serving == false"> <v-icon class="white--text">radio_button_checked</v-icon></span> 
              </v-toolbar-title>
            </v-toolbar>
            <v-flex xs-4></v-flex>
            <span class="display-4 text-xs-center"><span class="red--text"><strong>{{score2}}</strong></span>
              <br>
              <v-btn @click="score2 -= 1" class="red white--text">DECEREMENT</v-btn>
             <v-btn @click="addPoints(2)" class="green white--text">INCEREMENT</v-btn>
            </span>
            <v-flex xs-4></v-flex>
          </v-card>
          </v-flex>
          </v-layout>


        </v-card>
      </v-container>
    </v-content>

  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        clipped: false,
        drawer: true,
        fixed: false,
        items: [
          { icon: 'bubble_chart', title: 'Inspire' }
        ],
        miniVariant: false,
        right: true,
        rightDrawer: false,
        title: 'ScoreKeeper',
        score1: 0,
        score2: 0,
        p1name: '',
        p2name: '',
        editDialog: false,
        p1serving: false,
        winner: 0,
        gameOver: false
      }
    },

    methods: {
      clear() {
        this.p1name = ''
        this.p2name = ''
        this.score1 = 0
        this.score2 = 0
      },

      addPoints(player) {
        if (this.score1 >= 21 && (this.score1 - this.score2) >= 2 ) {
          this.winner = 1
          this.gameOver = true
        } else if (this.score2 >= 21 && (this.score2 - this.score1) >= 2 ) {
          this.winner = 2
          this.gameOver = true
        }

        if(player === 1) {
          this.score1++
        } else {
          this.score2++
        }
        this.checkServe()
      },

      checkServe() {
        if (this.score1 >= 20) {
          this.p1serving = false
        } else if (this.score2 >= 20) {
          this.p1serving = true
        } 
        else {
          if ((this.score1 + this.score2) % 5 == 0) {
            console.log("swap")
            this.p1serving = !this.p1serving
          }
        }
      }
    }
  }
</script>
