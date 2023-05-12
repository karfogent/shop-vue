<template>
  <div class="wrapper">

    <input v-model="inputValue" id="search-input"  placeholder="Введите ЖК / корпус / $№ квартиры / № паркинка ">
    <div style="display: flex; margin-top: 20px; align-items: center; cursor: pointer;">
      <div style="display: flex; margin-right: 20px">
        <check-box @click="changeMainBoxValue()" :check-box="this.mainBoxValue" v-on:click="selectAllCheckBoxes()" style="margin-right: 10px"/>
        <span style="font-family: sans-serif">все</span>
      </div>
      <delete-block />
    </div>

    <ul class="items-styles" style="margin-top: 10px">
      <li
          v-for="item in filteredItems"
          :key="item.id"
          class="item-block"
      >
        <check-box class="checkBoxesStyles" @click="changeOtherBoxValue(item)" :check-box="item.checkBoxValue"/>
        <!--   change padding-right in media -->
        <div class="media-padding" style="display: flex; justify-content: space-between;">
            <div class="items-width-block" style="display: flex; flex-direction: column;">
              <div style="display: flex; align-items: center;">
<!--   change margin-right in media -->
                <div style="color: #FF0D29; font-weight: 700; margin-right: 1vw; white-space: nowrap">{{ item.price }}</div>
                <parking-block style="width: 150px" v-if="item.hasParkingAttribute"/>
                <apartment-block style="width: 150px" v-if="item.hasApartmentAttribute"/>
              </div>
              <span>{{item.address}}, <span style="color: #808080">{{item.extraAddressInformation}}</span></span>

              <div style="display: flex; flex-direction: column;">
                <div class="media-max-width" style="margin-top: 3vh; display: flex; justify-content: space-between; align-items: center;">
                  <div style="line-height: 20px">
                    <span>{{item.apartmentNumber}}</span>
                    <div v-if="item.countRooms && item.apartmentNumber">
                      <span>{{item.countRooms}}</span>
                    </div>
                    <div v-else>
                      <span>{{item.alternativeApartmentInformation}}</span>
                    </div>
                  </div>

                  <div style="border-right: 2px solid #C4C4C4; height: 150%"></div>

                  <div style="line-height: 20px">
                    <span>{{item.apartmentArea}}</span>
                    <div v-if="item.floorNumber">
                      <span>{{item.floorNumber}}</span>
                    </div>
                  </div>
                </div>

                <div style="margin-top: 50px; display: flex;">
                  <map-icon id="map-icon" />
                  <!--   change max-width in media -->
                  <div style="margin-left: 5px;">{{item.additionalAddress}}</div>
                </div>
              </div>

            </div>

            <div style="display: flex; flex-direction: column; align-items: flex-end; height: 20vh">
              <img src="@/images/assigmentFromLegalPerson.png" class="imagesStatus" v-if="item.hasAssignmentFromLegalPerson">
              <img src="@/images/assigmentFromIndividual.png" class="imagesStatus" v-if="item.hasAssignmentFromIndividual">
              <img src="@/images/isBooked.png" class="imagesStatus" v-if="item.isBooked">
              <img src="@/images/isSold.png" style="width: 40%" v-if="item.isSold">
              <img id="house-plan" src="@/images/house-plan.png">
              <div style="color: #808080; position: relative; left: -30px">{{ item.timeAdded }}</div>
            </div>
          </div>
      </li>
    </ul>
  </div>

</template>

<script>
import checkBox from "@/components/checkBox";
import parkingBlock from "@/components/parkingBlock";
import deleteBlock from "@/components/deleteBlock";
import apartmentBlock from "@/components/apartmentBlock";
import mapIcon from "@/components/mapIcon";

export default {
  name: 'App',
  components: {
    checkBox, parkingBlock, deleteBlock, apartmentBlock, mapIcon},
  data() {
    return{
      inputValue: "",
      mainBoxValue: false,
      items: [
        {
          price: "7 733 300 руб.",
          address: "Чистое Небо",
          extraAddressInformation: "корпус 10, ||| кв. 2022 г.",
          image: "../images/house-plan.png",
          timeAdded: "Добавлено 21/11/2020",
          apartmentNumber: "кв.62",
          countRooms: "1 комнт.кв",
          apartmentArea: "234.38 м²",
          floorNumber: "7 этаж",
          additionalAddress: "Лен. область, Всеволожский район,д. Кудрово, ул. Столичная, д. 5, к. 1",
          hasParkingAttribute: false,
          hasApartmentAttribute: true,
          hasAssignmentFromIndividual: false,
          hasAssignmentFromLegalPerson: true,
          hasAssignmentFromIndividualForFilter: "",
          hasAssignmentFromLegalPersonForFilter: "Уступка от юр.лица",
          isBooked: false,
          isSold: false,
          checkBoxValue: false,
          id: 1
        },
        {
          price: "7 733 300 руб.",
          address: "Зеленый квартал на Пулковских высотах",
          extraAddressInformation: "корпус 10, ||| кв. 2022 г.",
          image: "../images/house-plan.png",
          timeAdded: "Добавлено 21/11/2020",
          apartmentNumber: "кв.62",
          countRooms: "1 комнт.кв",
          apartmentArea: "234.38 м²",
          floorNumber: "7 этаж",
          additionalAddress: "Комендантский пр., уч. 1 Каменка",
          hasParkingAttribute: false,
          hasApartmentAttribute: false,
          hasAssignmentFromIndividual: true,
          hasAssignmentFromLegalPerson: false,
          hasAssignmentFromIndividualForFilter: "Уступка от физ.лица",
          hasAssignmentFromLegalPersonForFilter: "",
          isBooked: false,
          isSold: false,
          checkBoxValue: false,
          id: 2
        },
        {
          price: "800 300 руб.",
          address: "Зеленый квартал на Пулковских высотах",
          extraAddressInformation: "корпус 10, ||| кв. 2022 г.",
          image: "../images/house-plan.png",
          timeAdded: "Добавлено 21/11/2020",
          alternativeApartmentInformation: "№ 7-10-2 (ПИБ №68)",
          apartmentArea: "15 м²",
          additionalAddress: "Ленинградская область, Всеволожский район, д. Кудрово, ул. Столичная, д. 5, к. 1",
          hasParkingAttribute: true,
          hasApartmentAttribute: false,
          hasAssignmentFromIndividual: false,
          hasAssignmentFromLegalPerson: false,
          hasAssignmentFromIndividualForFilter: "",
          hasAssignmentFromLegalPersonForFilter: "",
          isBooked: true,
          isSold: false,
          checkBoxValue: false,
          id: 3
        },
        {
          price: "800 300 руб.",
          address: "Зеленый квартал на Пулковских высотах",
          extraAddressInformation: "корпус 10, ||| кв. 2022 г.",
          image: "../images/house-plan.png",
          timeAdded: "Добавлено 21/11/2020",
          alternativeApartmentInformation: "№ 7-10-2 (ПИБ №68)",
          apartmentArea: "15 м²",
          additionalAddress: "Лен. область, Всеволожский район,д. Кудрово, ул. Столичная, д. 5, к. 1",
          hasParkingAttribute: true,
          hasApartmentAttribute: false,
          hasAssignmentFromIndividual: false,
          hasAssignmentFromLegalPerson: false,
          hasAssignmentFromIndividualForFilter: "",
          hasAssignmentFromLegalPersonForFilter: "",
          isBooked: false,
          isSold: true,
          checkBoxValue: false,
          id: 4
        }
      ]
    }
  },
  methods: {
    selectAllCheckBoxes() {
      this.items = this.items.map((item) => item.checkBoxValue === true ? {...item, checkBoxValue: this.mainBoxValue}
          : {...item, checkBoxValue: this.mainBoxValue} )
    },
    changeMainBoxValue(){
      this.mainBoxValue = this.mainBoxValue !== true
    },
    changeOtherBoxValue(item){
      item.checkBoxValue = item.checkBoxValue !== true
    },
  },
  computed: {
    filteredItems() {
      return this.items ? this.items.filter(el => {
        return (
            el.address?.toLowerCase().includes(this.inputValue?.toLowerCase())
        )
      }) : null;
    }
  }
}
</script>

<style scoped lang="scss">
*{
  box-sizing: border-box;
  font-family: sans-serif;
  font-size: 14px;
}

.wrapper{
  width: 80vw;
  margin: 0 auto;
}

input{
  width: 100%;
  border: 1px solid #C4C4C4;
  height: 38px;
  background: url("images/search.svg") no-repeat center left 3vw;
  text-align: center;
  margin-top: 89px;
  &::placeholder{
    padding-left: 6vw;
    text-align: left;
  }
  &:focus{
    &::placeholder {
      opacity: 0;
    }
  }
}
.items-styles {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(600px, 1fr));
  grid-column: 2 / -1;
  grid-gap: 15px;
  padding: 0;
  .item-block {
    height: 238px;
    background-color: #FFFFFF;
    list-style-type: none;
  }
}

@media screen and (max-width: 1495px) {
  .media-padding{
    padding: 10px 10px 10px 4.5vw;
  }
  .items-width-block{
    max-width: 40vw;
  }
  .media-max-width{
    width: 30vw;
  }
  .imagesStatus{
    width: 50%;
  }
  #house-plan{
    width: 200px;
  }
  .checkBoxesStyles{
    position: relative;
    top: 50%;
    left: 2vw;
  }
  #map-icon{
    width: 5%;
  }
}
@media screen and (min-width: 1495px) {
  .imagesStatus{
    width: 60%
  }
  .items-width-block{
    max-width: 25vw;
  }
  .media-max-width{
    width: 15vw;
  }
  .media-padding{
    padding: 10px 0 10px 3vw;
  }
  #house-plan{
    width: 200px;
  }
  .checkBoxesStyles{
    position: relative;
    top: 50%;
    left: 1vw;
  }
  #map-icon{
    width: 10%;
  }
}

@media screen and (max-width: 576px){
  .media-padding{
    padding: 10px 10px 10px 10vw;
  }
  .items-width-block{
    max-width: 75vw;
  }
  .media-max-width{
    width: 75vw;
  }
  .imagesStatus{
    width: 50%;
  }
  #house-plan{
    width: 200px;
  }
  .checkBoxesStyles{
    position: relative;
    top: 50%;
    left: 3vw;
  }
  #map-icon{
    width: 5%;
  }
  #search-input{
    width: 150vw;
  }
}
</style>
