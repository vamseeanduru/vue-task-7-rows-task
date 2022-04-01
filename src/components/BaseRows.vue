<template>
<div class="container-fluid">
  <div class="row">
    <div class="col-12">
      <div :style="{ position: divPosition }">
        <table class="table table-borderless table-sm mb-0">
          <thead>
            <tr>
              <th for="for row number"></th>
              <th for="row" class="row-table-header"></th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td style="width: 100%">
                <div class="lot-div" style="background: #ccc;height: 5px; display: flex; position: relative">
                  <div class="lot-div" ref="sliderLine" v-for="(rowLot, idx) in lots" :key="idx" :style="{background: rowLot.lotColorCode ,width:`${rowLot.Lot_length}%`, marginLeft:`${rowLot.Width_From}%`}"
                    style="height: 5px; position:absolute">
                    <h6 v-show="rowLot.showHeader" class="row-data" :id="`rowDiv-${idx}-${rowLot.Production_location_Id}-${rowLot.Lot_Id}-${idx}`">
                      <span v-for="column in rowLotHeading" :class="column.class" :key="column.name">{{ column.title }}{{ rowLot[column.name] }}</span>
                    </h6>
                  </div>
                </div>
              </td>
              <td style="display: flex;">
                <div @click="selectLotDiv(0)" class="boxes"></div>
                <input type="number" @input="enterNewLotQty(0, parseInt($event.target.value))" v-if="isLotSelected(0)"/>
                <div style="display: flex;" v-for="(lot, idx) in lots" :key="idx">
                  <div @click="selectLotDiv(idx+1)" :style="getBoxStyle(lot)"></div>
                  <input @input="enterNewLotQty(idx+1, parseInt($event.target.value))" type="number" v-if="isLotSelected(idx+1)"/>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: "BaseRows",
  props: {
    rowInfo: Object,
    rowLots: {
      type: Array,
      default () {
        return [];
      }
    },
    rowLotHeading: {
      type: Array,
      default () {
        return [{
          name: 'Lot_number',
          title: ''
        }];
      }
    },
  },
  created() {
    // console.log(this.rowLots);
  },
  data() {
    return {
      lots: this.rowLots,
      rowDimensions: {
        height: null,
        left: null,
        right: null,
        top: null,
        width: null
      },
      lotsSelected: [],
      divPosition: "relative",
      inputData: null,
    };
  },
  mounted() {
    this.rowDimensions = this.$refs.sliderLine[0].getBoundingClientRect();
  },
  methods: {
    enterNewLotQty(idx, val) {
      this.lots.push({
        Width_From: 0,
        Width_To: 100,
        Lot_length: 100,
        lotColorCode: 'red',
        No_of_Plants: val
      })
    },
    selectLotDiv(idx) {
      let lotIdx = this.lotsSelected.findIndex(x => x === idx);
      if (lotIdx === -1) this.lotsSelected.push(idx);
      else this.lotsSelected.splice(lotIdx, 1);
    },
    isLotSelected(idx) {
      return this.lotsSelected.includes(idx);
    },
    getBoxStyle(rowLots) {
      let Group_color_code = rowLots.Lot_Id !== null ? rowLots.Group_color_code : "#a4a4a4";
      // let additionalStyle = `-webkit-box-shadow: 0px 0px 5px 1px ${Group_color_code};-moz-box-shadow: 0px 0px 5px 1px ${Group_color_code};box-shadow: 0px 0px 5px 1px ${Group_color_code};`;
      let opacity = 0.5;
      return (
        `width:12px;height:12px;margin-left: 5px;background-color:${Group_color_code};opacity:${opacity};`
      );
    },
  },
};
</script>

<style scoped>
.table tr td {
  white-space: normal;
  color: #004085;
  border-color: #9fceff;
}


.boxes {
  background: #a4a4a4;
  float: left;
  height: 12px;
  width: 12px;
}

.parent-slider-div {
  width: 100%;
  display: flex;
  flex-direction: row;
  background-color: #ccc;
  height: 5px;
}

.parent-slider-div:hover {
  cursor: pointer;
}

.rowDataOnHover {
  position: relative;
  width: 20px;
  z-index: 1;
  height: 5px;
}

.lot-div {
  height: 5px;
}

.row-data {
  position: relative;
  bottom: 16px;
  color: black;
  width: fit-content;
  font-size: 12px;
}

.bold-text {
  font-weight: 700;
  font-size: small;
}

input {
  width: 4em;
  height: 22px;
  position: relative;
  bottom: 5px;
  margin-left: 5px;
  font-weight: 700;
  -moz-appearance: textfield;
}


input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>,
