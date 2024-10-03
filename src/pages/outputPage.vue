<template>
    <div>
        <div>
            {{venuedata}}
        </div>
        <br>
        <div>
            {{ highestW }}
        </div>
        <br>
        <div>
        {{ highScore}} 
       </div>
       <br>
       <div v-for="xyz in batters" :key="xyz">
        {{ xyz }}
        </div>
        <br>
       <div>
        {{ tosswinnername }}
       </div>
       <br>
       <div>
        {{ namesFull }}
       </div>
       <br>
       <br>
       <div>
        {{ confirm11 }}
       </div>
    </div>
    
</template>

<script>

import qwerty from '../data/data.json';

console.log("hi");

console.log(qwerty.Matchdetail.Venue.Name); 

    export default {

        data(){
        return{
        venuedata : qwerty.Matchdetail.Venue.Name,
        highScore : 0,
        highestW : 0,
        batters : [],
        tosswinnername : '',
        highestPR : 0,
        namesFull : [],
        confirm11 : []
        }
        },

        created(){
            this.highest(),
            this.mostWickets(),
            this.batting(),
            this.tossWinner(),
            this.confirm(),
            this.both()
        },

        methods:{

            highest(){
                let highScore = 0;
                qwerty.Innings.forEach((x)=> {
                    x.Batsmen.forEach((batsmen) =>{
                        

                        if(batsmen.Runs > highScore){
                            highScore = Number(batsmen.Runs);
                            console.log(highScore)
                        }

                        this.highScore = highScore;
                    })
                });

                
            },

            mostWickets(){
                let highestW = 0;
                qwerty.Innings.forEach((x)=> {
                    x.Bowlers.forEach((bowler) =>{
                        if(bowler.Wickets > highestW){
                            highestW = Number(bowler.Wickets);
                            console.log(highestW)
                        }

                        this.highestW = highestW;
                    })
                });

                
            },

            batting(){
                let batters = [];
                qwerty.Innings.forEach((x)=> {
                    x.Batsmen.forEach((batsmen) =>{
                        batters.push(batsmen.Batsman)
                    })
                });
                // console.log(batters);
            },

            confirm(){
                let confirm11 = [];
                for (let key in qwerty.Teams) {

                    for(let key1 in qwerty.Teams[key].Players){

                        if(qwerty.Teams[key].Players[key1].Confirm_XI){
                            this.confirm11.push(qwerty.Teams[key].Players[key1].Name_Full)
                        }
                        
                    }
                }
                
                console.log(confirm11);
            },
            
            tossWinner(){
                let tosswinnerid = qwerty.Matchdetail.Tosswonby
                this.tosswinnername = qwerty.Teams[tosswinnerid].Name_Full
                console.log(this.tosswinnername)

            },
            both(){
                let names = [];
                let highestPR = 0;

                qwerty.Innings.forEach((y) => {
                    y.Partnerships.forEach((x) => {
                    if(highestPR < x.Runs){
                        highestPR = x.Runs;
                        
                    }
                })
                })
                qwerty.Innings.forEach((y) => {
                    let select = y.Partnerships.filter(z => z.Runs == highestPR);

                    console.log(select);

                    select.forEach((x)=>
                    x.Batsmen.forEach((c)=>{
                        names.push(c.Batsman)
                    })
                )
                    console.log(select);
                })
                console.log(names);

                
                let namesFull = [];

                for(let x in qwerty.Teams){
                    let new1 = qwerty.Teams[x].Players;

                    console.log(new1);

                    for(let x in names)
                    for(let key in new1){
                        if(key == names[x])
                        this.namesFull.push((new1[key].Name_Full))
                    }       
                    console.log(namesFull)        
                }  
            }


        }
}
</script>

<style scoped>

</style>