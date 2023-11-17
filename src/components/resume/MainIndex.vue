<template>
    <main>
        <p>{{ labelVisual }}</p>
        <h1 :class="{'red': isNegative}" >{{ amountCurrency }}</h1>
        <div class="graphic">
            <slot name="graphic"/>  
        </div>
        <div class="action">
            <slot name="action"/>
        </div>
    </main>
</template>
<script>
    const currencyFormatter = new Intl.NumberFormat('es-CO', {
        style: 'currency',
        currency: 'COP'
    });

    export default {
        props:{
            totalLabel:{
                type:String,
            },
            label:{
                type:String,
                default: null,
            },
            totalAmount:{ 
                type: Number,
            },
            amount:{
                type:Number,
                default: null
            }
        },
        computed:{
            labelVisual(){
                return this.label !==null ?this.label :this.totalLabel;
            },
            amountVisual(){
                return this.amount !==null ?this.amount :this.totalAmount;
            },
            amountCurrency(){
                return currencyFormatter.format(this.amountVisual);
            },
            isNegative(){
                //console.log('this.amountVisual',this.amountVisual)
                return this.amountVisual < 0;
            }
        }
    }
</script>

<style scoped>
.red {
    color: red;
}
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}

h1,
p {
  margin: 0;
  text-align: center;
}

h1 {
  margin-top: 14px;
  color: var(--brand-green);
}

.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 48px 24px;
  box-sizing: border-box;
}
</style>