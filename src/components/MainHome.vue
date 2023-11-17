<template>  
    <MainLayout>
        <template #header>
            <MainHeader></MainHeader>
        </template>
        <template #resume>
            <Resume 
                :label="'Ahorro total'"
                :total-amount="totalAmount"
                :amount="amount"
                >
                <template #graphic>
                    <MainGraphic :amounts="amounts" @select="select"/>
                </template>
                <template #action>
                    <MainAction @create="create"/>
                </template>
            </Resume>
        </template>
        <template #movements>
            <Movements
                :movements="movements"
                @remove="remove"
            />

        </template>
    </MainLayout>
</template>

<script>
import MainLayout from './MainLayout.vue';
import MainHeader from './MainHeader.vue';
import Resume from './resume/MainIndex.vue';
import MainGraphic from './resume/MainGraphic.vue';
import MainAction from './MainAction.vue';
import Movements from './movements/MainIndex.vue';


export default {
    components: {
    MainLayout,
    MainHeader,
    Resume,
    Movements,
    MainAction,
    MainGraphic
    },
    data(){
        return {
            label: null,
            amount: null,
            //amounts: [100, 200, 500, 200, -400, -600, -300, 0, 300, 500],
            movements:[
               /*  {
                    id: 0,
                    title: 'Movimiento 1',
                    description: 'Lorem ipsum dlor sit amet',
                    amount: 100,
                    time: new Date('11-03-2023')
                }, */
               
            ]
        }
    },
    computed: {
        amounts(){
            const lastDays = this.movements
            .filter(m =>{
                const today = new Date();
                const oldDate = today.setDate(today.getDate() - 30);

                return m.time > oldDate;
            })
            .map(m => m.amount)

            //console.log(lastDays)

            return lastDays.map((m, i) => {
                const lastMovements = lastDays.slice(0,i + 1);
                //console.log(lastMovements);
                return lastMovements.reduce((suma, movement) => {
                    //console.log(lastMovements);

                    return suma + movement;
                },0)
            });
        },
        totalAmount(){
            return this.movements.reduce((suma, m) => {
                return suma + m.amount
            }, 0)
        }
    },
    mounted(){
        const movements = JSON.parse(localStorage.getItem('movements'));
        console.log('movements',movements);

        if(Array.isArray(movements)){
            this.movements = movements?.map(m => {
                return {...m, time: new Date(m.time)}
            });
        }

    },
    methods: {
        create(movement){
            this.movements.push(movement);
            this.save();
        },
        remove(id){
            const index = this.movements.findIndex(m => m.id === id);
            this.movements.splice(index,1);
            this.save();
        },
        save(){
            localStorage.setItem("movements",JSON.stringify(this.movements));
        },
        select(el){
            console.log(el);
            this.amount = el;
        }
    }
}

</script>