## Vuejs Error and Solution

This README provides the errors encountered during the project and their solutions.

---

### Vue Full Calendar Package - Adding Custom Holidays

- ![Error Icon](error_icon.png) **Problem:**
    - While implementing the Full Calendar package in one of my projects, there arose a need to incorporate custom holidays into the calendar. Unfortunately, the Full Calendar package doesn't offer a built-in solution for this requirement.

- **Solution:**
    - To address this, I devised the following strategy:
        1. Established a database section and table named 'holidays' to store holiday dates and occasions.
        2. Iterated through all date cells in the Full Calendar and compared them with the 'holidays' table's date field.
        3. Applied a custom CSS class to the calendar cells corresponding to holiday dates.
        4. Styled these holiday cells to distinguish them visually, ensuring user comprehension.
        5. Implemented a feature where clicking on a holiday cell triggers a message displaying the holiday occasion.
        6. I also used momnet.js here to format date, Its not mendatory. We can use vanilla javascript.

```javascript
data() {
    return {
        holidays: [], // Array to store holidays retrieved from backend response
        calendarOptions: {
            plugins: [
                dayGridPlugin,
                timeGridPlugin,
                listPlugin,
                interactionPlugin,
                multiMonthPlugin,
            ],
            initialView: "dayGridMonth",
           
            dateClick: this.handleDateClick,
            dayCellClassNames: this.handleDayCellClassNames,
        },
    };
},
methods: {
    isHoliday(date) {
        const formattedDate = moment(date).format("YYYY-MM-DD");
        const holiday = this.holidays.find(
            (holiday) => holiday.date === formattedDate
        );
        return holiday || false;
    },
    handleDayCellClassNames(arg) {
        if (this.isHoliday(arg.date)) {
            return "fc_holiday";
        }
        return null;
    },
    handleDateClick(info) {
        const holiday = this.isHoliday(info.dateStr);
        if (holiday) {
            this.showAlert({
                icon: "info",
                title: `This is a Holiday: ${holiday.occasion}`,
                text: "Please select an office day or time.",
                backdrop: `rgba(0,0,123,0.4)`,
            });
        }
    }
}
```

```css
/* Custom CSS for Full Calendar holidays */
.fc-daygrid-body .fc_holiday {
    opacity: 0.6;
}
.fc-daygrid-body .fc_holiday .fc-daygrid-day-top::after {
    position: absolute;
    content: "Holiday";
    right: 0;
    bottom: 0;
    font-weight: 600;
    background-color: #f2f1f9 !important;
    padding: 5px !important;
    box-shadow: 0px -5px 5px rgba(0, 0, 0, 0.1);
    margin-top: -1px;
    margin-left: -1px;
    border-radius: 6px !important;
    border-bottom-left-radius: 0 !important;
    border-top-right-radius: 0 !important;
}
```
