<script setup lang="ts">
import { computed, onBeforeMount, ref } from 'vue';
let ifMoved = ref(false);
let teamStats:Object =  {};
let rivalStats:Object = {};
const team:Object = {
        name: "Ivysaur",
        nickname: "",
        level: 16,
        hp: { base: 60, iv: 0, ev: 0 },
        atk: { base: 62, iv: 0, ev: 0 },
        def: { base: 63, iv: 0, ev: 0 },
        spAtk: { base: 80, iv: 0, ev: 0 },
        spDef: { base: 80, iv: 0, ev: 0 },
        speed: { base: 60, iv: 0, ev: 0 },
      };
const rival:Object = {
        name: "Charmeleon",
        nickname: "",
        level: 16,
        hp: { base: 58, iv: 0, ev: 0 },
        atk: { base: 64, iv: 0, ev: 0 },
        def: { base: 58, iv: 0, ev: 0 },
        spAtk: { base: 80, iv: 0, ev: 0 },
        spDef: { base: 65, iv: 0, ev: 0 },
        speed: { base: 60, iv: 0, ev: 0 },
      };

const damageCalculation = (
      level = 1,
      power = 1,
      atackStat = 1,
      defenseStat = 1,
      item = 1,
      critical = 1,
      multipleHit = 1,
      weather = 1,
      stab = 1,
      type = 1,
      random = 1) => {
        console.log(
        ((((2 * level) / 5 + 2 * power * (atackStat / defenseStat)) / 50) *
          item *
          critical +
          2) *
        multipleHit *
        weather *
        stab *
        type *
        random
      );
        ifMoved.value = true;
        console.log(ifMoved);
        
        setTimeout(() => {
          ifMoved.value = false;
          console.log(ifMoved);
        }, 1500)
      }
const calculateHp = (base, level) => {
  return (base * 2 * level) / 100 + level + 10;
}
const calculateStat = (base, level) => {
  return (base * 2 * level) / 100 + 5;
}
const calcTotalStat = (base) => {
  return {
        name: base.name,
        nickname: base.nickname,
        level: base.level,
        hp: calculateHp(base.hp.base, base.level),
        atk: calculateStat(base.atk.base, base.level),
        def: calculateStat(base.def.base, base.level),
        spAtk: calculateStat(base.spAtk.base, base.level),
        spDef: calculateStat(base.spDef.base, base.level),
        speed: calculateStat(base.speed.base, base.level),
      };
}
onBeforeMount(() => {
  teamStats = calcTotalStat(team);
  rivalStats = calcTotalStat(rival);
})

</script>

<template>
  <div>
    <div class="battle">
      <div class="rival">
        <span>
          {{rival.name}} {{rivalStats.hp}}
        </span>
        <img
          :class="{damaged: ifMoved}"
          src="../../node_modules/pokemon-sprites/sprites/pokemon/other/showdown/5.gif"
          alt=""
        />
      </div>
      <div class="team">
        {{ifMoved}}
        <img
          :class="{shake: ifMoved}"
          src="../../node_modules/pokemon-sprites/sprites/pokemon/other/showdown/back/2.gif"
          alt=""
        />
        <span>
          {{team.name}}{{teamStats.hp}}
        </span>
      </div>
    </div>
    <div class="dialogBox">
      <div class="moveBox">
        <div>
          <button class="moves" @click="damageCalculation(teamStats.level, 40, teamStats.atk, rivalStats.def)">Tackle</button>
          <button class="moves" @click="damageCalculation(teamStats.level, 40, teamStats.atk, rivalStats.def, 1, 1, 1, 1, 1.5, 0.5)">Vine whip</button>    
        </div>
        <div>
          <button class="moves" @click="damageCalculation(teamStats.level, 60, teamStats.atk, rivalStats.def)">Struggle</button>
          <button class="moves" @click="damageCalculation(teamStats.level, 80, teamStats.atk, rivalStats.def, 1, 1, 1, 1, 1.5, 0.5)">Razor leaf</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.battle {
  background-image: url("../assets/Background1.png");
  height: 280px;
  width: 500px;
  margin-bottom: 5px;
}
.dialogBox {
  display: flex;
}
.moves{
  width: 50%;
}
.rival {
  position: relative;
  top: 40%;
  left: 55%;
}
.team {
  position: relative;
  top: 45%;
  left: 20%;
}
.moveBox {
  width: 60%;
}
.shake {
  animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  transform: translate3d(0, 0, 0);
}
.damaged {
  animation: damaged 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  transform: translate3d(0, 0, 0);
}

@keyframes shake {
  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}

@keyframes damaged {
  0%{opacity: 1;}
  25%{opacity: .5;}
  50%{opacity: 0;}
  75%{opacity: .5;}
  100%{opacity: 1;}
}
</style>
