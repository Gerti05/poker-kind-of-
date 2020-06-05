<template>
  <div>
    <button @click="[randamize(), ante()]">Deal</button>
    <div v-if="player1">{{ player1 }}</div>
    <button
      @click="check()"
      v-if="player1 && playerTurn === 'firstPlayer' && chipsPlayer2 > 0"
    >Check</button>
    <button
      @click="oneFourthPot()"
      v-if="player1 && playerTurn === 'firstPlayer' && chipsPlayer2 > 0"
    >1/4 Pot</button>
    <button @click="betPlayer1()" v-if="player1 && playerTurn === 'firstPlayer'">1/2 Pot</button>
    <button @click="betPlayer1()" v-if="player1 && playerTurn === 'firstPlayer'">3/4 Pot</button>
    <button @click="betPlayer1()" v-if="player1 && playerTurn === 'firstPlayer'">All In</button>
    <p>{{ chipsPlayer1 }}</p>
    <div style="display: inline-block">
      <p v-if="runout.flop[0]">
        {{ runout.flop[0] + " " + runout.flop[1] + " " + runout.flop[2] + " " }}
        <span v-if="turnPhase">{{ runout.turn[0] + " " }}</span>
      </p>
    </div>

    <div v-if="player2">{{ player2 }}</div>
    <button
      @click="check()"
      v-if="player2 && playerTurn === 'secondPlayer' && chipsPlayer2 > 0"
    >Check</button>
    <button
      @click="oneFourthPot()"
      v-if="player2 && playerTurn === 'secondPlayer' && chipsPlayer2 > 0"
    >1/4 Pot</button>
    <button @click="betPlayer1()" v-if="player2 && playerTurn === 'secondPlayer'">1/2 Pot</button>
    <button @click="betPlayer1()" v-if="player2 && playerTurn === 'secondPlayer'">3/4 Pot</button>
    <button @click="betPlayer1()" v-if="player2 && playerTurn === 'secondPlayer'">All In</button>
    <p>{{ chipsPlayer2 }}</p>
    <div v-if="player1 && player2">{{ pot }}</div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      playersArray: [],
      pot: 0,
      chipsPlayer1: 100,
      chipsPlayer2: 100,
      player1bets: 0,
      playerTurn: "firstPlayer",
      runout: {
        flop: [],
        turn: [],
        river: []
      },
      flopPhase: true,
      turnPhase: false,
      riverPhase: false
    };
  },
  methods: {
    randamize: function() {
      let randomNum1 = Math.floor(Math.random() * 52);
      let randomNum2 = Math.floor(Math.random() * 51);
      let randomNum3 = Math.floor(Math.random() * 50);
      let randomNum4 = Math.floor(Math.random() * 49);
      let flop1 = Math.floor(Math.random() * 48);
      let flop2 = Math.floor(Math.random() * 47);
      let flop3 = Math.floor(Math.random() * 46);
      let turn = Math.floor(Math.random() * 45);
      let river = Math.floor(Math.random() * 44);
      this.pokerDeck(
        randomNum1,
        randomNum2,
        randomNum3,
        randomNum4,
        flop1,
        flop2,
        flop3,
        turn,
        river
      );
    },
    pokerDeck: function(num1, num2, num3, num4, flop1, flop2, flop3, turn) {
      let deckArray = [
        "AH",
        "AD",
        "AS",
        "AC",
        "KH",
        "KD",
        "KS",
        "KC",
        "QH",
        "QD",
        "QS",
        "QC",
        "JH",
        "JD",
        "JS",
        "JC",
        "10H",
        "10D",
        "10S",
        "10C",
        "9H",
        "9D",
        "9S",
        "9C",
        "8H",
        "8D",
        "8S",
        "8C",
        "7H",
        "7D",
        "7S",
        "7C",
        "6H",
        "6D",
        "6S",
        "6C",
        "5H",
        "5D",
        "5S",
        "5C",
        "4H",
        "4D",
        "4S",
        "4C",
        "3H",
        "3D",
        "3S",
        "3C",
        "2H",
        "2D",
        "2S",
        "2C"
      ];
      let firstCard = deckArray[num1];
      let secondCard = deckArray.filter(card => card != firstCard)[num2];
      let thirdCard = deckArray.filter(
        card => card != firstCard && card != secondCard
      )[num3];
      let fourthCard = deckArray.filter(
        card => card != firstCard && card != secondCard && card != thirdCard
      )[num4];
      let firstFlopCard = deckArray.filter(
        card =>
          card != firstCard &&
          card != secondCard &&
          card != thirdCard &&
          card != fourthCard
      )[flop1];

      let secondFlopCard = deckArray.filter(
        card =>
          card != firstCard &&
          card != secondCard &&
          card != thirdCard &&
          card != fourthCard &&
          card != firstFlopCard
      )[flop2];
      let thirdFlopCard = deckArray.filter(
        card =>
          card != firstCard &&
          card != secondCard &&
          card != thirdCard &&
          card != fourthCard &&
          card != firstFlopCard &&
          card != secondFlopCard
      )[flop3];
      let turnCard = deckArray.filter(
        card =>
          card != firstCard &&
          card != secondCard &&
          card != thirdCard &&
          card != fourthCard &&
          card != firstFlopCard &&
          card != secondFlopCard &&
          card != thirdFlopCard
      )[turn];
      this.playersArray = [
        firstCard + " " + thirdCard,
        secondCard + " " + fourthCard
      ];
      this.runout.flop = [firstFlopCard, secondFlopCard, thirdFlopCard];
      this.runout.turn = [turnCard];
    },
    ante: function() {
      this.chipsPlayer1 -= 5;
      this.chipsPlayer2 -= 5;
      this.pot = 10;
    },
    check: function() {
      if (this.playerTurn === "firstPlayer" && this.flopPhase === true) {
        this.playerTurn = "secondPlayer";
      } else if (
        this.playerTurn === "secondPlayer" &&
        this.flopPhase === true &&
        this.turnPhase === false
      ) {
        this.turnPhase = true;
        this.playerTurn = "firstPlayer";
      }
    },
    oneFourthPot: function() {
      let oneFourth = parseFloat((this.pot * 0.25).toFixed(2));
      if (this.playerTurn === "firstPlayer" && this.chipsPlayer1 >= oneFourth) {
        this.chipsPlayer1 = (this.chipsPlayer1 - oneFourth).toFixed(2);
        this.pot = parseFloat((this.pot + oneFourth).toFixed(2));
        this.playerTurn = "secondPlayer";
        console.log("h");
      } else if (
        this.playerTurn === "firstPlayer" &&
        this.chipsPlayer1 < oneFourth
      ) {
        console.log("g");
        this.allIn();
      } else if (
        this.playerTurn === "secondPlayer" &&
        this.chipsPlayer1 >= oneFourth
      ) {
        this.chipsPlayer2 = (this.chipsPlayer2 - oneFourth).toFixed(2);
        this.pot = parseFloat((this.pot + oneFourth).toFixed(2));
        this.playerTurn = "firstPlayer";
        console.log("f");
      } else if (
        this.playerTurn === "secondPlayer" &&
        this.chipsPlayer1 < oneFourth
      ) {
        this.allIn();
        console.log("d");
      }
    },
    allIn: function() {
      if (this.playerTurn === "firstPlayer") {
        this.pot = parseFloat(
          (this.pot + parseInt(this.chipsPlayer1)).toFixed(2)
        );
        this.chipsPlayer1 = (this.chipsPlayer1 - this.chipsPlayer1).toFixed(0);
        this.playerTurn = "secondPlayer";
      } else if (this.playerTurn === "secondPlayer") {
        this.pot = parseFloat(
          (this.pot + parseInt(this.chipsPlayer2)).toFixed(2)
        );
        this.chipsPlayer2 = (this.chipsPlayer2 - this.chipsPlayer2).toFixed(0);
        this.playerTurn = "firstPlayer";
      }
    }
  },
  computed: {
    player1: function() {
      return this.playersArray[0];
    },
    player2: function() {
      return this.playersArray[1];
    }
  }
};
</script>

<style></style>
