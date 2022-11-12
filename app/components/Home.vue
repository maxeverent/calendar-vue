<template>
    <Page>
        <ActionBar>
            <Label text="Calendar"/>
        </ActionBar>
        <StackLayout class="body">
            <StackLayout  class="body-calendar">
                <FlexboxLayout class="nav">
                    <Button text="<-" @tap="beforePage" class="button" />
                    <StackLayout>
                        <Label>{{calendar.name_mounth}}</Label>
                        <Label class="now-year">{{calendar.year}}</Label>
                    </StackLayout>
                    <Button text="->" @tap="nextPage" class="button" />
                </FlexboxLayout>
                <Flexboxlayout>
                    <StackLayout v-for="weekDay, index in weekDays" class="column">
                        <Label class="label" style="background: #918e8e">{{weekDay.nameDay}}</Label>
                        <StackLayout v-if="sortDaysWeek(numberPage, weekDay.num, index) == false">
                            <Label text=" " class="label-empty" />
                        </StackLayout>
                        <StackLayout>
                            <Label 
                                v-for="day in calendar.days" 
                                v-if="weekDay.num === getWeekDay(numberPage, day)"
                                :class="{labelnowdate: nowDateClass(day, numberPage)}"
                                class="label"
                            >{{day}}</Label>                                                    
                        </StackLayout> 
                    </StackLayout> 
                </Flexboxlayout>
            </StackLayout>
            <Button text="Back" @tap="backToNowMounth" class="back-button" />
        </StackLayout>
    </Page>
</template>

<script>

  export default {
    data() {
        return {
            nowYear: new Date().getFullYear(),
            numberPage: new Date().getMonth(),
            nowDay: new Date().getDate(),
            nowMonth: new Date().getMonth(),
            weekDays: [{num: 1 , nameDay: "Mo"}, {num: 2 , nameDay: "Tu"},
                {num: 3 , nameDay: "We"}, {num: 4 , nameDay: "Th"}, {num: 5 , nameDay: "Fr"}, 
                {num: 6 , nameDay: "Sa"}, {num: 0 , nameDay: "Su"}, ],
            calendar: null,
            mounths: ['January', 'February', 'Mart', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
        }
    },
    methods: {
        setCalendar() {
            let days = new Date(this.nowYear, this.numberPage + 1, 0).getDate()
            //this.calendar = null
            this.calendar = {mounths: this.numberPage, name_mounth: this.mounths[this.numberPage], days: days, year: this.nowYear}
            console.log(this.calendar)
        },
        nextPage() {
            if (this.numberPage === 11) {
                this.numberPage = 0
                this.nowYear++
            }
            else {
                this.numberPage += 1
            }       
        },
        beforePage() {
            if (this.numberPage === 0) {
                this.numberPage = 11
                this.nowYear--
            }
            else {
                this.numberPage -= 1
            } 
        },
        getWeekDay(mounth, day) {
            const test = new Date(this.nowYear, mounth, day).getDay()
            return test           
        },
        sortDaysWeek(mounth, num, index) {
            let status = null
            for (let i = 1; i < index + 2; ++i) {
                if (num === this.getWeekDay(mounth, i)) {
                    status = true
                    break
                }
                else {
                    status = false
                }
            }
            return status
        },
        backToNowMounth() {
            let nowMounth = new Date().getMonth()
            let nowYear = new Date().getFullYear()
            this.numberPage = nowMounth
            this.nowYear = nowYear
             
        },
        nowDateClass(day, numberPage) {
            if (day === this.nowDay && numberPage === this.nowMonth) {
                return true
            }
            return false
        }
    },
    created() {
        this.setCalendar()
    },
    watch: {
        numberPage() {
            this.setCalendar()
        },
        nowYear() {
            this.setCalendar()
        }
    }
  };

</script>

<style src="../style.css"></style>
