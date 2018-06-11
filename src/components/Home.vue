<template>
  <div class="home">
    <div class="container">
      <div class="card">
        <div class="card-content">
          <ul>
            <li class="team-row" v-for="(team, index) in teams" :key="index">{{ `${ team.name }: ${ team.score } ` }}
              <span v-if="team.winner">Win!</span>
            </li>
          </ul>
          <button @click="play">Go</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      teams: [
        { name: 'Qued', attack: 8, defense: 7, score: 0, winner: false },
        { name: 'Dave', attack: 4, defense: 9, score: 0, winner: false }
      ],
      gameOver: false,
      attackerIndex: 0,
      defenderIndex: 1
    }
  },
  methods: {
    play () {
      // toggle attacker
      this.attackerIndex = this.attackerIndex === 0 ? 1 : 0;
      this.defenderIndex = this.defenderIndex === 0 ? 1 : 0;

      // get rolls
      const attackRoll = chance.integer({ min: 0, max: this.teams[this.attackerIndex].attack })
      const defenderRoll = chance.integer({ min: 0, max: this.teams[this.defenderIndex].defense })

      // attacker must be not tied
      if (attackRoll > defenderRoll) {
        this.teams[this.attackerIndex].score += 1;
      }

      // if attacker gets 7 points, end it
      if (this.teams[this.attackerIndex].score === 7) {
        this.teams[this.attackerIndex].winner = true
        this.gameOver = true
      } else {
        setTimeout(this.play, 1000)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

  .home .team-row {
    font-size: 1.5em;
  }

</style>
