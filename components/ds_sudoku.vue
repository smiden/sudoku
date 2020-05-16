<template>
  <div>
      <ul class="sudoku"> 
         <template v-for='(rows, i) in bodySudoku[0]'>
           <template v-for='(column, j) in rows'>
             <li :key='Math.random(j + 1)' @click='selectCell(i, j, $event)'>
              <span :key='Math.random(i + 1)' :class="{'colorFontOk': (!column.default && !column.error),
              'colorFontError': (!column.default && column.error),
              'borderSudokuRig':(j === 2 || j === 5), 
              'borderSudokuBot': (i === 2 || i === 5)}">
                  {{ ( column.value === 0 ) ? '' : column.value }}
              </span> 
             </li>  
           </template>
         </template>
         <div id='numz' v-if='inputShow'>
          <template v-for='(move, k) in moves'>
             <div @click="addValueCell(move)" :key='k'>{{ move }}</div>
          </template>
      </div>        
      </ul>  
  </div>
</template>

<script>

// import json from '@/assets/server/json'

export default {
    data(){
        return{
            data: [[3,0,1,2,0,0,0,0,0],[0,0,7,0,0,0,5,0,0],[0,0,0,8,0,0,0,0,0],[5,0,0,4,6,0,0,0,0],[0,0,0,0,0,0,0,2,1],[0,0,0,0,0,0,0,0,0],[0,0,0,0,7,0,6,0,0],[0,8,0,0,0,0,0,3,0],[0,2,0,0,0,1,0,0,0]],
            inputShow: false,
            sellActive: [],
            bodySudoku: [[]],
            moves:[1,2,3,4,5,6,7,8,9,'Очистить'],     
            level: [
               {name: 'Легко', value: 4, status: true },
               {name: 'Средне', value: 3, status: false},
               {name: 'Сложно', value: 2, status: false}
         ],
        }
    },
    created(){
        this.startSudoku();
    },
    methods: {
        startSudoku(){
           let level = this.level.find((item) => (item.status) ? item : false);
           this.fillInSudoku();

            this.bodySudoku[0].forEach((item, j) => { 
                for(let i = 0; i < 8; i++){
                    if(this.data[j][i] !== 0) { 
                        item[i].value = this.data[j][i]; 
                        item[i].default = true; 
                    }       
                }
            })
        },
        fillInSudoku(){
          for(let i = 0; i < 9; i++){
                this.bodySudoku[0][i] = [];
              for(let j = 0; j < 9; j++){
                  this.bodySudoku[0][i].push({value: 0, default: false, error: false});
              }
          }
        },
        checkCell(row, column, valueCell){ 
            let checkRow = this.bodySudoku[0][row].find((item) => item.value === valueCell);
            let checkColumn = this.bodySudoku[0].find((item) => item[column].value === valueCell)
            let checkSquare = true;

                outer: 
                for(let i = row - row % 3; i < row - row % 3 + 3; i++){
                 for(let j = column - column % 3; j < column - column % 3 + 3; j++){
                    if(this.bodySudoku[0][i][j].value === valueCell){
                        checkSquare = false;
                        break outer;
                    }
                 }
            }

            if(checkRow === undefined && checkColumn === undefined && checkSquare)
                      return true
                return false
        },
        checkFullSudoku(){
            let result = false;

            outer:
            for(let j = 0; j < this.bodySudoku[0].length;j++){
                for(let i = 0; i < 8; i++){ 
                    if(this.bodySudoku[0][j][i].value === 0 || this.bodySudoku[0][j][i].error){
                        result = true
                        break outer;
                    }
                }
            }
            if(result)
               return false
            alert('Игра пройдена');
        },
        selectCell(row, column, event){ 
             if(this.bodySudoku[0][row][column].default) event.preventDefault();
             else {
                this.sellActive = Array(row, column);
                this.inputShow = true;  
             }

        },
        addValueCell(value){
               value = (value === 'Очистить' ? 0 : value);
               this.bodySudoku[0][this.sellActive[0]][this.sellActive[1]].error = !this.checkCell(this.sellActive[0], this.sellActive[1], value);
               this.bodySudoku[0][this.sellActive[0]][this.sellActive[1]].value = value;
               this.checkFullSudoku();
               this.inputShow = false;
        }
    },
}
</script>

<style lang="scss">
    .sudoku {
     position: relative;
     border-style: solid;
     border-color: #000;
     border-width: 1px 2px 2px 1px;
     width: 360px;
     height: 360px;
     margin: 0 auto;
     padding: 0;
     list-style: none;
        li {
            border-left: 1px solid #000;
            border-top: 1px solid #000;
            cursor: pointer;
            width: 39px;
            height: 39px;
            float: left;
            padding: 0;
            user-select: none;
            line-height: 130%;
            display: list-item;
            text-align: -webkit-match-parent;
            span {
                display: list-item;
                width: 40px;
                height: 40px;
                line-height: 40px;
                font-size: 30px;
                font-family: georgia, serif;
                font-variant-numeric: lining-nums;
                text-align: center;
                overflow: hidden;
                background-repeat: no-repeat;
                user-select: none;
            }
        }
    }
    ui {
        display: block;
        list-style-type: disc;
        margin-block-start: 1em;
        margin-block-end: 1em;
        margin-inline-start: 0px;
        margin-inline-end: 0px;
        padding-inline-start: 40px;
    }

    #numz {
       display:flex;
	   flex-direction: rows;
       flex-wrap: wrap;
       width: 98px;
       height: 66px;
       background: white;
       border: 1px solid #CCC;
       position: absolute;
       left: 110px;
       top:70px;
       div {
             background: #ccc;
           text-align: center;
           height: 20px;
           width: 20px;
           padding: 6px;
           border: .3px solid black;
           cursor: pointer;
           z-index: 1000;
           &:last-child {
               width: 100%;
               text-align: center;
           }
       }
    }
    .borderSudokuRig {
       border-right: 2px solid black;
    }
    .borderSudokuBot {
       border-bottom: 2px solid black;
    }
    .colorFontOk {
        color: #42b983;
    }
    .colorFontError {
        color: red;
    }
</style>

