# JavaScript Date & Time - 30 Real-Time Scenario Questions

## Beginner Level

### 1. Meeting Reminder System
**Scenario:** You're building a meeting reminder app. Create a function that takes a meeting date/time and returns how many hours are left until the meeting starts.

**Example:** If meeting is tomorrow at 2 PM and current time is today 3 PM, return "23 hours remaining"

---

### 2. Age Calculator
**Scenario:** Build a user profile feature that displays a user's exact age in years. Given a birthdate, calculate their current age accounting for whether their birthday has passed this year.

**Example Input:** Birthdate: January 15, 1995  
**Expected Output:** 30 years old (or 29 if birthday hasn't occurred yet this year)

---

### 3. Business Hours Checker
**Scenario:** Create a function for a restaurant website that checks if they're currently open. Operating hours: Monday-Friday 9 AM to 9 PM, Saturday-Sunday 10 AM to 11 PM.

**Expected Output:** "We're Open!" or "We're Closed. Opens at [time]"

---

### 4. Countdown Timer
**Scenario:** Build a New Year countdown that shows days, hours, minutes, and seconds remaining until January 1st of next year.

**Expected Output:** "305 days, 14 hours, 23 minutes, 45 seconds until New Year!"

---

### 5. Event Date Formatter
**Scenario:** You're displaying events on a website. Format dates to show "Today", "Tomorrow", "Yesterday", or the actual date if it's more than 2 days away.

**Example:** If event is today → "Today at 5:30 PM"  
If event is in 3 days → "October 20, 2025 at 5:30 PM"

---

## Intermediate Level

### 6. Time Zone Converter
**Scenario:** Create a world clock widget that converts a given time from one timezone to another. User selects "New York 3 PM" and wants to know the time in Tokyo.

**Input:** Time: 3:00 PM, From: America/New_York, To: Asia/Tokyo  
**Expected Output:** "Tokyo: 4:00 AM (next day)"

---

### 7. Subscription Expiry Tracker
**Scenario:** Build a subscription management system that shows when a user's subscription expires and whether it's expired, expiring soon (within 7 days), or active.

**Example:** Subscription ends on October 20, 2025  
**Output:** "Expiring in 3 days - Renew Now!"

---

### 8. Work Hours Logger
**Scenario:** Create a timesheet application that calculates total hours worked in a week. Users clock in and out multiple times per day.

**Input:**  
- Monday: 9:00 AM - 5:30 PM  
- Tuesday: 8:45 AM - 6:15 PM  

**Expected Output:** Total hours worked this week

---

### 9. Date Range Validator
**Scenario:** For a hotel booking system, validate that check-out date is after check-in date and both dates are in the future. Also calculate the number of nights.

**Example:** Check-in: October 25, 2025, Check-out: October 28, 2025  
**Output:** "3 nights - Valid Booking"

---

### 10. Recurring Event Generator
**Scenario:** Build a calendar feature that generates all occurrences of a weekly meeting for the next 3 months. Meeting happens every Tuesday at 10 AM.

**Expected Output:** Array of all Tuesday 10 AM dates for next 3 months

---

### 11. Birthday Reminder
**Scenario:** Create a system that alerts users of upcoming birthdays in their contact list within the next 7 days, sorted by how soon they occur.

**Input:** List of contacts with birthdates  
**Expected Output:** "John's birthday in 2 days (October 19), Sarah's birthday in 5 days (October 22)"

---

### 12. Appointment Slot Generator
**Scenario:** Generate available 30-minute appointment slots for a doctor between 9 AM and 5 PM, excluding lunch break (1-2 PM) and already booked slots.

**Input:** Date: October 18, 2025, Booked: ["10:00 AM", "2:30 PM"]  
**Expected Output:** Array of available time slots

---

### 13. Late Fee Calculator
**Scenario:** Library system that calculates late fees. Books are due after 14 days. Late fee is $0.50 per day. Calculate the fee for a book borrowed on September 15, 2025 and returned today.

**Expected Output:** Days overdue and total late fee

---

### 14. Quarter Reporter
**Scenario:** Financial app needs to determine which fiscal quarter a transaction belongs to and show quarter start/end dates.

**Input:** Transaction Date: August 15, 2025  
**Expected Output:** "Q3 2025 (July 1 - September 30)"

---

### 15. Session Timeout Warning
**Scenario:** Implement a session timeout feature that warns users 2 minutes before their 30-minute session expires. Show a countdown in the warning.

**Expected Behavior:** After 28 minutes of inactivity, show "Session expires in 2:00"

---

## Advanced Level

### 16. Smart Date Parser
**Scenario:** Parse natural language date inputs like "next Friday", "in 3 weeks", "last Monday", "2 days ago" into actual JavaScript Date objects.

**Input:** "next Friday at 3pm"  
**Expected Output:** Date object for the upcoming Friday at 3 PM

---

### 17. Shift Scheduler
**Scenario:** Create a rotating shift schedule for a 24/7 operation with 3 shifts (morning 6 AM-2 PM, evening 2 PM-10 PM, night 10 PM-6 AM). Generate a 2-week schedule for 5 employees ensuring each works 5 days per week.

**Expected Output:** Complete schedule with no overlaps or conflicts

---

### 18. Meeting Overlap Detector
**Scenario:** Build a calendar conflict detector that checks if a new meeting overlaps with existing meetings, considering different time zones for remote participants.

**Input:** New meeting: 2 PM EST (1 hour), Existing: 1:30 PM EST (1 hour)  
**Expected Output:** "Conflict detected: Overlaps with existing meeting by 30 minutes"

---

### 19. Dynamic Price Calculator
**Scenario:** Movie theater charges different prices based on time: matinee (before 5 PM): $8, evening (5-9 PM): $12, late night (after 9 PM): $10. Weekend surcharge +$3. Calculate ticket price for given showtime.

**Input:** Saturday, October 18, 2025, 7:30 PM  
**Expected Output:** "$15 (Evening + Weekend)"

---

### 20. Work Anniversary Tracker
**Scenario:** HR system that calculates years of service and determines if today is an employee's work anniversary (exact date).

**Input:** Hire Date: October 17, 2020  
**Expected Output:** "5 years of service today! 🎉"

---

### 21. Daylight Saving Time Handler
**Scenario:** Create a function that detects when DST changes occur and adjusts scheduled events accordingly. A weekly meeting at 9 AM should remain at 9 AM local time even after DST change.

**Expected Behavior:** Handle the "spring forward" and "fall back" transitions correctly

---

### 22. Flight Duration Calculator
**Scenario:** Calculate actual flight duration accounting for time zones. Flight departs NYC at 8 PM EST and arrives in London at 8 AM GMT next day.

**Expected Output:** "Flight duration: 7 hours"

---

### 23. Age-Gated Content
**Scenario:** Streaming service needs to verify if a user is 18+ to watch certain content. Account for leap years and ensure they've actually reached their 18th birthday.

**Input:** Birthdate: October 18, 2007  
**Expected Output:** true/false for access permission

---

### 24. Performance Review Scheduler
**Scenario:** Generate performance review dates for all employees. Reviews happen every 6 months from their start date. Flag reviews due within next 30 days.

**Input:** Employee start date: April 15, 2023  
**Expected Output:** Next review: April 15, 2025 (Due in 180 days) or "UPCOMING!" if within 30 days

---

### 25. Historical Date Range Search
**Scenario:** E-commerce platform needs to find all orders within a custom date range and group by week/month/quarter based on user selection.

**Input:** Start: Jan 1, 2025, End: Oct 17, 2025, Group by: Month  
**Expected Output:** Order counts grouped by month

---

## Expert Level

### 26. Multi-Timezone Meeting Finder
**Scenario:** Find optimal meeting time for team members across 5 different time zones (PST, EST, GMT, IST, JST) during their respective business hours (9 AM - 6 PM local time).

**Expected Output:** Common available time slots that work for all time zones

---

### 27. Ramadan/Lunar Calendar Integration
**Scenario:** Build a feature that identifies Islamic holidays (based on lunar calendar) and adjusts business operations accordingly. Note: Lunar months are ~29.5 days.

**Challenge:** Lunar calendar dates shift ~11 days earlier each solar year

---

### 28. Payroll Date Calculator
**Scenario:** Calculate bi-weekly payroll dates for entire year. If payday falls on weekend/holiday, move to previous business day. Include handling for bank holidays.

**Input:** First payday: January 10, 2025 (Friday), Holidays: [list of bank holidays]  
**Expected Output:** All 26 payroll dates for 2025 with weekend/holiday adjustments

---

### 29. SLA Compliance Tracker
**Scenario:** Customer support system with SLA: respond within 2 business hours, resolve within 24 business hours (Mon-Fri, 9 AM-5 PM only). Calculate if ticket is breaching SLA.

**Input:** Ticket created: Friday 4:30 PM  
**Expected Output:** Response due: Monday 10:30 AM, Resolution due: Tuesday 4:30 PM

---

### 30. Historical Data Aging Policy
**Scenario:** Implement data retention policy that archives records older than 7 years but must preserve data for ongoing legal cases. Also account for leap years in age calculation.

**Input:** Record date: October 17, 2018, Legal hold: false  
**Expected Output:** "Archive eligible in 0 days" or "Retained due to legal hold"

---

## Bonus Tips for Solving These Scenarios

1. **Always use `new Date()` carefully** - It can be timezone-dependent
2. **Consider edge cases**: leap years, month-end dates, DST transitions
3. **Use UTC when appropriate** - Prevents timezone-related bugs
4. **Libraries to consider**: date-fns, moment.js (legacy), dayjs, luxon
5. **Test with different timezones** - Use `Intl` API for timezone handling
6. **Remember months are 0-indexed** - January is 0, December is 11
7. **Account for leap seconds** - Rare but important for high-precision apps
8. **Validate user input** - Invalid dates can cause unexpected behavior